---
layout: post
title: "Stealthy BASHing"
date: 2012-09-10 11:42
comments: true
categories: 
---

Want to keep sensitive data out of your BASH history? Set this environment variable in 
`.bash_profile` or `.bashrc`, then prepend a space to any sensitive command you type
and it will not show up in your history.

    export HISTCONTROL=ignoreboth
    
Alternatively here's a one-off approach that prompts you for your secret value:

    heroku config:add SECRET=`read x; echo $x`