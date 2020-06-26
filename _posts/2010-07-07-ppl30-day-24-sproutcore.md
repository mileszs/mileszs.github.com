---
layout: post
title: "PPL30 Day 24: SproutCore"
date: 2010-07-07 22:14:10 -0400
comments: true
categories: [ppl30, programming]
---
I thought I would dive in to SproutCore today. Though I'm not convinced SproutCore is ultimately the answer for "How do we build blazingly fast desktop-class web applications?" it definitely seems like a good idea, and a good place to start as far pushing the envelope goes.

<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

## Installation ##

    gem install sproutcore

## Resources ##

### SproutCore Website ###

Of course, [the SproutCore website](http://www.sproutcore.com/) is a fantastic place to start. It's pleasant to look at, easy to navigate, and has plenty of resources at which to look. 

### SproutCore Wiki ###

There is _plenty_ of information in [the SproutCore wiki](http://wiki.sproutcore.com) to look at. In fact, I recommend following along with the [Todos tutorial](http://wiki.sproutcore.com/Todos%C2%A0Intro).

## What I Think I Now Know ##

SproutCore is a web framework that helps to create desktop-like applications by putting all the work into the client. SproutCore projects are written using Javascript. The whole thing is ultimately compiled down to a static file that is HTML, CSS, and Javascript, so it can be hosted virtually anywhere. If you need to store data, you can create a RESTful back-end for SproutCore to query (written using whatever your favorite back-end language might be). Most everyone wants to see the various controls and UI elements possible with SproutCore. Well, you can [do that on the SproutCore demo site](http://demo.sproutcore.com/).

As I was working through an example, I thought you might like to see a view, which I found confusing. How are views confusing? Views appear to be simply Javascript templates (actually, they're Javascript objects, more-or-less -- data files, in other words, that describe how views should be configured. Here's a quick example:

<% coderay(:lang => :javascript, :line_numbers => :inline) do -%>
  mainPane: SC.MainPane.design({

    childViews: 'middleView topView bottomView'.w(),

    topView: SC.ToolbarView.design({
      layout: { top: 0, left: 0, right: 0, height: 36 },
      anchorLocation: SC.ANCHOR_TOP
    }),

    middleView: SC.ScrollView.design({
      hasHorizontalScroller: NO,
      layout: { top: 36, bottom: 32, left: 0, right: 0 },
      backgroundColor: 'white',
      contentView: SC.ListView.design({
      })

      layout: { bottom: 0, left: 0, right: 0, height: 32 },
      anchorLocation: SC.ANCHOR_BOTTOM
    }),

    bottomView: SC.ToolbarView.design({

      layout: { bottom: 0, left: 0, right: 0, height: 32 },

      anchorLocation: SC.ANCHOR_BOTTOM

    })
  })
<% end %>


They do allow for a lot flexibility and power.

You know what I ultimately realized? I've got to spend a _lot_ more time with SproutCore for it to be worth anything. It's so different from what I'm accustomed, trying to speed through a tutorial with one eye closed (man, I'm tired) is completely useless.

I do find SproutCore to be intriguing -- perhaps more intriguing than I thought it would be before I started today. Now I just have to think of an app to develop with SproutCore! If you'd like to see what other people have built using SproutCore, [check out some Real Applications using SproutCore](http://wiki.sproutcore.com/Projects-Using-SproutCore). MobileMe is one!
