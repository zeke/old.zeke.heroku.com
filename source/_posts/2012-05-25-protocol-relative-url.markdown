---
layout: post
title: "Double-Slash?"
date: 2012-05-25 10:41
comments: true
categories: 
---

{% img no-border /images/extra/http-double-slash.jpg double-slash %}

Cruising through some heroku source code today, I came across a stylesheet link tag that 
looked something like this:

    <link href='//nav.heroku.com/header.css' media='all' rel='stylesheet' type='text/css'>
    
Notice the funky double-slash at the beginning of the URL. Weird, right? I'd never seen it either. 
It's a **protocol-relative URL**. [Paul Irish explains:](http://paulirish.com/2010/the-protocol-relative-url/)

*"If the browser is viewing that current page in through HTTPS, then it'll request that asset with the 
HTTPS protocol, otherwise it'll typically request it with HTTP. This prevents that awful "This Page 
Contains Both Secure and Non-Secure Items" error message in IE, keeping all your asset requests 
within the same protocol."*