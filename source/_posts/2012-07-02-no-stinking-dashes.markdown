---
layout: post
title: "No Stinking Dashes"
date: 2012-07-02 14:48
comments: true
categories: 
---

jQuery (as of 1.7.2) doesn't allow dashes in HTML `data-` attribute names, so you're better off
using underscores instead. Check out [this jsfiddle](http://jsfiddle.net/xhSPQ/) for evidence.

<a href="http://jsfiddle.net/xhSPQ/">
  {% img no-border /images/extra/no-stinking-dashes.png %}
</a>

And while we're on the topic, I discovered today that Haml now supports a `:data` attribute. 
Give it a (flat) ruby Hash and each of the key/value pairs will be transformed into a custom HTML5 data attribute.
[Huzzah!](http://haml.info/docs/yardoc/file.HAML_REFERENCE.html#html5_custom_data_attributes)
