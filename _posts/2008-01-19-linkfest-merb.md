--- 
layout: post
title: LinkFest - Merb
date: 2008-01-19 03:18:56 UTC
comments: true
categories: 
--- 
[Merb](http://merbivore.com/) is a Ruby web framework, not unlike Rails. In fact, you might call it Rails Lite (although, I'm not positive either party would appreciate that comparison). Merb was built, from the beginning, to be lean and fast. It was built with Mongrel and Erb at the basis, hence the name. Merb has a pretty small footprint, but in playing with it recently, it's is still pretty powerful, as well as flexible. (Damn, I sound like a marketing guy.)

## Enough Sales Talk\!

One way in which it is flexible is that it doesn't install with an ORM, like ActiveRecord, as a dependency. There are three ORMs officially supported at the moment: ActiveRecord, DataMapper, and Sequel. Chances are that you already know about ActiveRecord. I would call DataMapper ActiveRecord Lite, but, again, I don't know if that's what everyone would like to hear. One small thing I really love about DataMapper is the ability to express the model's properties within the model, which will subsequently create the database table.

There really is much more to Merb. I'm sure I will have more to say in the future. File uploads are easy and fast. Using some other "templating language" is easy, and I've been loving learning Haml and Sass. It currently defaults to Erubis (faster Erb, from what I understand), and support for Markaby is baked in as well. A much better and clearer (and prettier) highlight reel is available here: [Rethink - Merbtastic](http://rethink.unspace.ca/2007/12/3/merb-tastic).

## Why should I take Merb seriously?

Did I mention that Merb is written by [Ezra Zygmuntowicz](http://brainspl.at/) from [Engine Yard](http://engineyard.com/)? The same engine yard that recently took a few million in VC funding. The same engine yard that has thrown around 5 developers at Rubinius. The same Engine Yard that much of 'The Industry' has begun taking *much* more seriously. They have some of the best, if not the best Rails hosting in existence. Why should you take Merb seriously? Oh, you mean, other than all of the cool stuff? Because it has some ***serious*** support behind it. It's *going* places.

## Where is it going?

The Merb guys [recently announced](http://yehudakatz.com/2008/01/14/merbnext/) that Merb is splitting up. Not in a *bad* way, in a good way\! Like when Ben Affleck and Jennifer Lopez split up, showing mercy for all of us that wanted to hack our ears off every time we heard the term "Bennifer". Merb is going to become 'merb-core' and 'merb-more' (man, they're clever bastards). The merb-core gem will be just the very basics for running web services and simple sites. Essentially, it's going to get even more modular. The point release before its 1.0 release will be 0.9, and should include most of the 'new stuff'.

One of the 'new things' that I want to highlight is that Merb will no longer be solely dependent on Mongrel. Don't get me wrong, Mongrel is *freakin' awesome*. However, there is a new little server creeping around on our block. It's in a simiilar spirit to Mongrel. In fact, it utilizes the Mongrel parser, as well as Event Machine, and Rack. It's called [Thin](http://code.macournoyer.com/thin/), and it is not only *freakin' awesome*, but it's also *freakin' fast*\! It'll talk more about Thin in the next couple days.

There are other frameworks out there, as well. There is one in particular (Ramaze), that I would like to test out pretty soon, as it already runs on Ruby 1.9, and is evidently extremely speedy, as well. That said....

## Merb is *Freakin' Awesome*

Merb is small. Merb is light. Merb is fast. Merb is for realz.

## Uh, that was a LinkFest?

Oh\! By the way, the article [merb + datamapper + noob: quick start](http://codingbitch.com/p/comboy/merb%20+%20datamapper%20+%20noob:%20quick%20start) could help you get started with Merb. If you don't believe it when *I* tell you why you should care about Merb, [Luke at Rails Spikes does a better job](http://railspikes.com/2007/4/1/merb), and also gives a better intro, and probably writes better. I also recommend just going through [del.icio.us bookmarks](http://del.icio.us/search/?fr=del_icio_us&p=merb&type=all). It's fun. Good luck\!
