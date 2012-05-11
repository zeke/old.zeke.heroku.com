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
    
![heroku scheduler](http://f.cl.ly/items/2X0j1u010E3T0e1S1N0h/Screen%20Shot%202012-05-11%20at%2012.23.18%20AM.png)

Put this in your Rakefile and smoke it. I mean push it.

    require "net/http"

    desc "Ping app"
    task :ping do
      url = 'my-app.herokuapp.com'
      puts "ping? (#{url})"
      r = Net::HTTP.new(url, 80).request_head('/')
      puts "pong! (#{r.code} #{r.message})"
    end


Sha. Push it. Push it reeeaall good.
