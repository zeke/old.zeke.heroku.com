---
layout: post
title: "To Branch Or Not To Branch"
date: 2012-05-16 22:52
comments: true
categories: 
---

I recently started an email thread in search of "the heroku way" of git branching. I got some good feedback:

**[@pvh](https://twitter.com/#!/pvh)**: 
"Don't branch! If you have to branch keep it short (like, a day or two tops). In general, everything you do (everywhere!) should be designed to roll out incrementally in many small steps. [...] To be fair, we don't want 'branches', so much as we want transient named pull requests that use branches as a transport mechanism."

**[@michaelahale](https://twitter.com/#!/michaelahale)**: 
"I prefer short-lived feature branches. That provides an opportunity for review before committing to master. I'm pretty sure
everyone would agree that we do **not** want [[the nvie branching model]](http://nvie.com/posts/a-successful-git-branching-model/)."

**[@ped](https://twitter.com/#!/ped)ro**: 
"I think even with incremental roll outs branches are still useful.
At the API team we normally use branches for anything that is not
trivial or that deserves a review. The changes on the branch most of
the time are still designed to be rolled out incrementally, but having
a branch and pull request allows us to review those changes before
they get to master (and as we move close to automated deploys having
master always green/deployable is crucial)."

**[@glenngillen](https://twitter.com/#!/glenngillen)**: 
"Probably a good summation: [scottchacon.com/2011/08/31/github-flow.html](http://scottchacon.com/2011/08/31/github-flow.html)"