---
layout: post
title: My New Home
date: 2009-02-07 23:54:29 -0500
comments: true
categories:
---
I *finally* did it. I switched to a static blog, powered by [Webby](http://webby.rubyforge.org/). I have been [talking about doing this](/2008/07/10/too-many-choices.html) since July of 2008. Hey, I came in under a year\! Sweet\!

### Git and Webby, Sitting in a Tree

![](/images/kissing-squirrels-tree.jpg)

Essentially, Webby and [Git](http://git-scm.com) power this blahg. I tell Webby I want to post a new entry. When finished, I commit it and push it to the “central” repository on my server. A post-recieve hook pulls the update into the web-accessible directory. BAM\! It’s like magic.

So, basically, this:

```
webby blog:post my-new-home
```

`my-new-home.txt` is opened in my $EDITOR (VIM\!), with the proper meta-data already filled in according to Webby rules.

After I finish writing:

```
webby build
git commit -a -m “POST: My New Home”
git push origin master
```

It’s that simple.

Well, it could be, anyway. In reality, I’m still tweaking design aspects, and I am still tweaking some content. From time to time, in the midst of working on a post, I will find that some other aspect of the site needs tweaked. I like to see what my post looks like after nearly every paragraph because I have some sort of mild OCD or something. Thankfully, I can simply run `webby autobuild` in the background. This launches an instance of [WEBrick](http://www.webrick.org/), opens your browser, and points it at `localhost:4331`. Whenever a page is saved, it updates the relevant output, and serves it up. This has the added benefit of not needing to run `webby build` before committing. It’s fan-freakin’-tastic.

## JS-Kit Comments

### The Good

I went with [JS-Kit Comments](http://js-kit.com/comments) over Disqus (or Intense Debate, etc.). Look at the [feature list](http://js-kit.com/comments/features.html) yourself. There are a **ton** of features: OpenID integration (and Facebook Connect), Akismet SPAM protection, full custimization…. The list is lengthy. Check it out.

The first thing that struck me about JS-Kit was their site. While perhaps not as pretty as the Disqus site, the site proved a lot easier to find the exact information for which I was looking.

It also helps that I have gotten helpful replies on Twitter from [JSKitSupport](http://twitter.com/JSKitSupport). There are several people from JS-Kit on Twitter, and they are pretty active. I know, Disqus does this, too. They never contacted me, though.

JS-Kit has several other widgets to go with their Comments system.

### The Not-So-Good

Honestly, the name ‘JS-Kit Comments’ sucks. It sounds like some half-assed javascript library from 2001. Their logo isn’t great. Disqus is a [YCombinator](http://ycombinator.com) company, and people freakin’ love them some YC.

Give me a couple months, and I will probably be able to produce more bad aspects of JS-Kit with validity.

### The Other

I have not gotten my comments from soyunperdedor.com transferred yet. Sorry.

## New Domain(s)

I’ve decided to use a more obvious domain ‘mileszs.com’. (I do also own mileszs.org, and mileszs.net, which both hit soyunperdedor.com at the time of this writing. They won’t always.) While the domain soyunperdedor.com still makes me giggle a bit, it wasn’t very practical. It was difficult for some to remember, cumbersome for some to spell, and made no sense unless you either 1) know Spanish, or 2) know [Beck](http://www.last.fm/music/Beck/_/Loser).

![](/images/beck-mellow-gold.jpg)

> “You know, like in that Beck song, ‘**Loser**’.” <br />“*Excuse* me?\! Screw you, too, *buddy*.”

Soon enough, soyunperdedor.com will redirect to mileszs.com.

## The Design

Not much to talk about with regards to the design. The most interesting aspect probably is that I am using the [960 Grid System](http://960.gs/). I will admit that I never dove in and used [Blueprint CSS](http://www.blueprintcss.org/). I did use [YUI Grids CSS](http://developer.yahoo.com/yui/grids/) a bit. I found 960 to be easier with which to work compared to YUI, and I liked the grid and naming philosophies better than Blueprint. I think it’s a matter of opinion.

## The End

That’s it. I get to [blog like a hacker](http://tom.preston-werner.com/2008/11/17/blogging-like-a-hacker.html), although not using Mr. Preston-Werner’s [Jekyll](http://github.com/mojombo/jekyll/tree/master). (Webby is a bit more powerful, and supports [Sass](http://haml.hamptoncatlin.com/docs/rdoc/classes/Sass.html) and [Haml](http://haml.hamptoncatlin.com/), which is **huge**. Jekyll is good, though. I use it for [Vim Links of Worth and Valor](http://mileszs.github.com/), my GitHub Pages page.) I really love doing everything from the command line, instead of copying and pasting into a web form, or something equally as lame. Of course, there’s the added benefit of it not doing a *single* database request. So, it’s fast. It scales. It brings all the boys to the yard. *Damn right*, it’s better than yours.
