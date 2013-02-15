---
layout: post
title: "Keeping your unidyno humming"
date: 2012-05-11 00:12
comments: true
categories:
---

If you've got a single-dyno app that keeps winding down from inactivity
and consequently taking a long time to restart, you can use the free
[heroku scheduler](https://addons.heroku.com/scheduler) addon to keep it alive.

    heroku addons:add scheduler:standard
    heroku addons:open scheduler

Add a scheduler job that uses curl to ping your app every ten minutes:

    curl http://my-app.herokuapp.com

Here's what it looks like:

![heroku scheduler](http://cl.ly/image/1h3A3I0l2A3w/content#.png)