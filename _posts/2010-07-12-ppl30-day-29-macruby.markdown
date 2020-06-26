--- 
title:      "PPL30 Day 29: MacRuby"
layout:     post
alias: /blog/2010/07/12/ppl30-day-29-macruby.html
date: 2010-07-13 20:42
categories: [ppl30, programming]
--- 

Somehow I missed MacRuby when I covered JRuby and Ruby 1.9.2. (Let's ignore the fact that I also missed Rubinius _completely_.) MacRuby is a Ruby implementation on OSX technologies (Obj-C, etc.). It's an implementation of Ruby 1.9 (as opposed to 1.8.x), to be clear.

<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

## Installation ##

    rvm install macruby

## Resources ##

### Introduction to MacRuby ###

The obvious place to start is [the official introductory tutorial](http://www.macruby.org/documentation/tutorial.html)! This tutorial states up front that you don't need to know Objective-C, but it does in fact have several examples that are either in Objective-C, or are in reference to how the same would be done in Objective-C. In this way, a current _NSCoder_ should feel quite comfortable with this tutorial from the start. 

### Blog Post for MacRuby 0.6 ###

Yes, [the blog post announcing MacRuby 0.6](http://www.macruby.org/blog/2010/04/30/macruby06.html) is a good source of information. It is a good source of information because it covers the MacRuby debugger. One can run a programming using the `macrubyd` executable, which will drop into the debugger immediately. It is modeled after `gdb`. There is plenty more information contained in this blog post, too. I found myself getting excited about MacRuby, try as I might to avoid such feelings that may lead to **fanboyism**. Oh, I'm sorry. **[[NSFanboyism]]**.

### Getting Started with HotCocoa ###

HotCocoa is a framework that ships with MacRuby. Its intention is to make it easier to construct user interfaces programmatically, rather than resorting to using Interface Builder. Though this is not the only tutorial, the [Getting Started](http://www.macruby.org/hotcocoa/getting_started.html) tutorial written by dj2 on MacRuby.org is, as you can imagine, a good place to start.

### More Resources ###

Check out the [Learning about MacRuby](http://www.macruby.org/documentation.html) section at MacRuby.org. It has links to several more resources, including several screencasts.

## What I Think I Now Know ##

Well, I covered most of the cool stuff while discussing the articles I browsed in the **Resources** section. The HotCocoa Tutorial series of articles, the first of which I linked to above, is pretty interesting stuff. For someone who has not used it before, Interface Builder was a bit difficult to wrangle out of the gate, as is instructed towards the end of [the MacRuby intro](http://webby.rubyforge.org/). For whatever reason, text makes more sense to me than pretty buttons and drag-and-drops. (I understand Interface Builder can be pretty badass, once you get the hang of it. I think I heard that. Right?) I was able to get a window opened with stuff in it pretty easily in either one, but HotCocoa feels more natural for me. (That is unsurprising since you generate an app with a predefined directory structure, like you might a Rails app. Then you mess with some configuration real quick. Then you get to coding. It's comfortable.)

The MacRuby debugger seems more-or-less like ruby-debug. It's not necessarily something new. However, the fact that it was mentioned -- touted, even -- gave me the feeling that it is a bit more of a first-class citizen in MacRuby, whereas I think that ruby-debug gets neglected a bit. 

As of version 0.6, MacRuby is passing 85% of RubySpecs. It's able to run a modified version of Rails 3. That was the end of April. The previous release, 0.5, was released at the end of January. Hopefully that means, say, end of July for 0.7. However, between 0.4 and 0.5 there were nearly 11 months. It's not as if a pattern has been established.

Curious about its speed in comparison to other Ruby implementations? Antonio Cangiano has been covering the Ruby benchmark front for a while now. He [benchmarks MacRuby 0.6 against 1.8.7 and 1.9.1](http://programmingzen.com/2010/05/16/benchmarking-macruby-0-6/), and gets some interesting results. Benchmarking it against other Ruby implementations is an interesting exercise to me, but not for the usual reasons. To me, the reason for such benchmarks are to help decide what Ruby to run server-side. MacRuby's focus is certainly not on serving web apps. I have yet to meet someone who uses Mac servers in any serious capacity in the first place. (I'll admit that my having not met them doesn't mean they don't exist, or aren't surprisingly prevelant. I mean, they don't, and they aren't, but, you know, don't take my word for it.) More importantly, what are the performance repercussions of writing a desktop app in MacRuby versus writing it using Objective-C, if any?

If you're a Rubyist, and a Mac user, MacRuby really is pretty cool. I'm rarely interested in using desktop applications if I can find a good web app, so _writing_ desktop apps is certainly unappealing. However, being able to use Ruby makes it a bit more tempting, and lowers my personal barrier to entry into the OSX development club.

I would like to mention that MacRuby.org was built using [Webby](http://webby.rubyforge.org/), the same Ruby library that generates this site. Not a big deal, but it's nice to know I'm not the only one who still loves [Webby](http://webby.rubyforge.org/). [_Ed. note: I am running Octopress now._]

* * *

Expect the final entry, Day 30: Scheme, as soon as I can bang it out!
