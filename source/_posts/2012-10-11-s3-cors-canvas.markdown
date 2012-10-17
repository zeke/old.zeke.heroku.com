---
layout: post
title: "Tips for setting up CORS on S3"
date: 2012-10-11 10:08
comments: true
categories: 
---

I've been working on a project that uses an awesome Javascript library called [Color Thief](http://lokeshdhakar.com/projects/color-thief/)
to extract a color palette from an image. Now that [S3 has CORS support](http://aws.amazon.com/about-aws/whats-new/2012/08/31/amazon-s3-announces-cross-origin-resource-sharing-CORS-support/),
it's easier than ever to do client-side manipulation of user-uploaded images using Canvas.
In this post I'll cover some of the things that tripped me up along the way.

Check out the
[demo on heroku](http://cors-example.herokuapp.com/)
and the
[demo source on github](https://github.com/zeke/cors-example).

<a href="http://cors-example.herokuapp.com/">
  {% img http://f.cl.ly/items/2n3X080M272Q0B0K1r2E/Screen%20Shot%202012-10-16%20at%205.35.28%20PM.png %}
</a>

Testing with curl
-----------------

Setting up CORS on your S3 bucket can be accomplished pretty easily using the 
[S3 management console web interface](https://console.aws.amazon.com/s3). It looks like this:
<a href="https://console.aws.amazon.com/s3">
  {% img no-border http://f.cl.ly/items/2e3A3Z1m3I3t1v3y0i2t/Screen%20Shot%202012-10-16%20at%203.54.44%20PM.png %}
</a>

Once you've got your bucket configured,
you can use `curl` to verify that your CORS configuration is
working. Simply set an an origin header when making the request:

    curl -H "Origin: http://example.com" --verbose \
      http://cors-example.s3.amazonaws.com/rainbow.jpg
      

You should see the following response headers:

    < HTTP/1.1 200 OK
    < x-amz-id-2: E1JFVQnxrtk0riqK8q2G6RTlMh4zyxC/OTuV5AEUUDcCa9AXXL/C00iX2i5dmjZW
    < x-amz-request-id: AD2BA8F701742FD8
    < Date: Tue, 16 Oct 2012 22:30:25 GMT
    < Access-Control-Allow-Origin: http://example.com
    < Access-Control-Allow-Methods: GET
    < Access-Control-Max-Age: 3000
    < Access-Control-Allow-Credentials: true

Use the bucket-as-subdomain S3 URL format
-----------------------------------------

S3 URLs come in two flavors: domain-style (bucket.s3.amazonaws.com) 
and path-style (s3.amazonaws.com/bucket). In order for CORS to work,
your must use domain-style URLs.

    http://cors-example.s3.amazonaws.com/rainbow.jpg # Good
    http://s3.amazonaws.com/cors-example/rainbow.jpg # Bad
    
Don't use periods in your bucket names
--------------------------------------

Using a period in the bucket name name may confuse your browser and/or S3's interpretation
of the subdomain, so it's probably best to just avoid it.

Run a Development Server
------------------------

Most browsers won't let you access files using the file:// protocol.
To get around this, use python's simpleServer to run a local server 
on port 8000:

    python -m SimpleHTTPServer
    
Some browsers give localhost different privileges than regular domains, so open
your server at [lvh.me:8000](http://lvh.me:8000) instead of localhost:8000.
lvh.me is a domain that somebody bought and pointed at 127.0.0.1 expressly for
this purpose. How nice of them!

Refresh Often
-------------

Google Chrome has a tendency to cache images for a bit, regardless of the site from which you're requesting them.
Sometimes you have to force-refresh when you see this error in Chrome's Javascript console:

    Cross-origin image load denied by Cross-Origin Resource Sharing policy.

Don't Taint the Canvas
----------------------

When attempting to read data from an image on another domain using a Canvas function like 
`getImageData` (which tools like Color Thief rely on),
**you can't read the data if the image is already present in the DOM**. Instead you must
use Javascript to create a new DOM element, then assign its `crossOrigin` and `src`
attributes, as depicted in
[this coffeescript snippet](https://github.com/zeke/cors-example/blob/master/public/coffee/app.coffee):
      
    $ ->
      $('#rainbow').load ->  
        img = new Image
        canvas = document.createElement("canvas")
        ctx = canvas.getContext("2d")
        img.crossOrigin = "Anonymous"
        img.src = this.getAttribute('src')

        img.onload = =>
          canvas.width = img.width
          canvas.height = img.height
          ctx.drawImage(img, 0, 0)
          for rgb in createPalette(img, 5)
            color = "rgb(#{rgb.join(", ")})"
            $("#colors").append("<li style='background-color:#{color}'></li>")      


Further Reading
---------------

Here are a handful of links that helped me arrive at this working solution:

- [developer.mozilla.org/en-US/docs/CORS_Enabled_Image](https://developer.mozilla.org/en-US/docs/CORS_Enabled_Image)
- [blog.chromium.org/2011/07/using-cross-domain-images-in-webgl-and.html](http://blog.chromium.org/2011/07/using-cross-domain-images-in-webgl-and.html)
- [javascript.mfields.org/2011/creating-an-image-in-javascript](http://javascript.mfields.org/2011/creating-an-image-in-javascript/)
- [stackoverflow.com/questions/12812572/trouble-enabling-cors-on-s3](http://stackoverflow.com/questions/12812572/trouble-enabling-cors-on-s3)