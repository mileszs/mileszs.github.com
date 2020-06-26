--- 
layout: post
title: LinkFest - Thin
date: 2008-01-24 01:47:04 UTC
comments: true
categories: 
--- 
Many of you have likely heard of (and use) [Mongrel](http://mongrel.rubyforge.org/), the HTTP library and server for Ruby. However, you may not have heard of the new kid on the block, [Thin](http://code.macournoyer.com/thin/). (It's got the Right Stuff\! Sorry, couldn't resist.)

Thin is a Ruby web server that can operate as a drop-in replacement for Mongrel. Indeed, early benchmarks have shown it to be faster than Mongrel\! It marries three exceptional pieces of software, [the Mongrel parser](http://www.zedshaw.com/tips/ragel_state_charts.html), [Event Machine](http://rubyeventmachine.com/), and [Rack](http://rack.rubyforge.org/).

## Hype, hype, hype, tell me something else

It's simple. It's fast. It can be dropped in to many frameworks/situations and perform wonderfully. You can, right now, install it and use it as your development server (instead of Webrick or Mongrel). That is a great way to test it, get to know it. Who knows? Maybe the two of you will hit it off\! I wish you the best.

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sudo gem install thin  
  
...  
  
thin start

</div>

</div>

The pretty benchmark:

<div class="centerize">

![](http://soyunperdedor.com/files/linkfest_thin_chart.png)

</div>

## Are You Going to Continue Parroting Everyone Else?

Several other people have covered this much better than I am able. It would probably be in the best interest of both myself and you, the reader, for me to just list some links and let you go at it. I could get back to hacking up this Merb app I've been working on (more on that later), and you can quit reading my monotonous article. Right?

It's cool to see some of the activity at the Google Group: [thin-ruby](http://groups.google.com/group/thin-ruby/). Thin is actively developed, and has had several releases in the past couple weeks. Anyone willing can hack on it, so you're welcome to check out the git repository as well.

More articles:

  - [Thin - A web server that is really fast](http://blog.rayngwf.com/2008/01/thin-ruby-web-server-that-is-really.html)
  - [Current Uptime of the Thin Server Process Behind This Site](http://www.deze9.com/jp/blog/post?p=current-uptime-of-the-thin-server-process-behind)
  - [Thin + Nginx with Rails](http://glauche.de/2008/01/12/thin-nginx-with-rails/)
  - [How Many Thin Server Instances Are Best?"](http://glauche.de/2008/01/14/how-many-thin-server-instances-are-best/)
  - [Thin Web Server for Ruby Rocks\!](http://www.almostserio.us/articles/2008/01/11/thin-web-server-for-ruby-rocks) (check out the comment, too)
  - [List of Sites using Thin in Production](http://code.macournoyer.com/thin/users/)

There's a lot of potential in this thing. In fact, in the next version of Merb, the framework will be flexible enough that you can easily switch from Mongrel to Thin. I hope, at that time, to tell you how I did it (and how easy it was\!).
