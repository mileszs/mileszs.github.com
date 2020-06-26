--- 
layout: post
title: Simple Sparklines in Merb
date: 2008-02-05 03:48:51 UTC
comments: true
categories: 
--- 
Basic reporting with [Sparklines](http://nubyonrails.com/pages/sparklines) in Ruby web application frameworks is pretty simple. It is especially simple if you are using Ruby on Rails, and the available Sparklines plugin. For some, it may not be quite as clear if you wish to *not* use the plugin, or if you are using one of the other frameworks, such as Merb.

I have been trying to take a moment, when I can, to build something with Merb. It's been slow going. (Part of that problem is that, when I do have some time, I find myself quite easily distracted. I mean, I can't even be relied upon to churn out some rambling prose in this space once a week\!)

I am having a lot of fun with it. I am building a little application to track my weight losses (or gains) over time. As you can guess, it is *very* basic, but that's all I personally need. I would like to be able to see a little line graph of how I'm doing, though. It is extremely convenient to have your self-esteem ruined by a mere glance at a web page. Other applications take some reading of actual numbers\! In this way, I can be utterly discouraged, and contemplating death-by-overeating before I've even finished my triple grande carmel mocha (560 calories?).

## How can I get started, already?

First of all, you should know that [Mr. Topfunky](http://topfunky.com/) does a fine job of explaining first the "Why" and then the "How" in using Sparklines for reporting in [Better Reporting with Sparklines](http://nubyonrails.com/articles/better-reporting-with-sparklines). Again, we're talking about much more experienced and knowledgeable writer and programmer here. Check it out. That said, I hope you'll bear with me. I mean, even junior high basketball teams have a B-team, right?

## Oh, Smart Blahgging: Send Me to Someone Else's Site

Sorry, we'll just get straight to it. Install the gem:

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

sudo gem install sparklines

</div>

</div>

Seriously, generating sparklines is simple. It really is. The first thing you will want is a controller to handle the sparklines reports.

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

script/generate controller reports

</div>

</div>

We want to require sparklines in the controller.

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

<span style="color: #CC0066; font-weight: bold;">require</span> 'rubygems'  
<span style="color: #CC0066; font-weight: bold;">require</span> 'sparklines'  
  
<span style="color: #9966CC; font-weight: bold;">class</span> Reports \< Application  
<span style="color: #9966CC; font-weight: bold;">end</span>

</div>

</div>

Next is the meat of the this tip: generating the graph. Similar to Mr. Topfunky, I put the code in the show method of my Reports controller. Essentially, we want this method to return the image data.

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

<span style="color: #CC0066; font-weight: bold;">require</span> 'rubygems'  
<span style="color: #CC0066; font-weight: bold;">require</span> 'sparklines'  
  
<span style="color: #9966CC; font-weight: bold;">class</span> Reports \< Application  
   
  <span style="color: #9966CC; font-weight: bold;">def</span> show  
    graph = Sparklines.<span style="color: #9900CC;">plot\_to\_image</span><span style="color: #006600; font-weight: bold;">(</span>current\_user.<span style="color: #9900CC;">weights</span>,  
      <span style="color: #006600; font-weight: bold;">{</span>     
        :background\_color =\> 'transparent',  
        :step =\> <span style="color: #006666;">30</span>,  
        :height =\> <span style="color: #006666;">30</span>,  
        :line\_color =\> <span style="color: #996600;">"\#ffcccc"</span>,  
        :underneath\_color =\> <span style="color: #996600;">"\#ebf3f6"</span>  
      <span style="color: #006600; font-weight: bold;">}</span><span style="color: #006600; font-weight: bold;">)</span>  
    graph = graph.<span style="color: #9900CC;">to\_blob</span>  
  <span style="color: #9966CC; font-weight: bold;">end</span>  
<span style="color: #9966CC; font-weight: bold;">end</span>

</div>

</div>

## Simpler, please?

Notice, I didn't called 'render'. I just return 'graph = graph.to\_blob'. I end up calling this controller method within an image tag. Yup, that's right\! (This is in HAML, in case you hadn't noticed....)

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

%img<span style="color: #006600; font-weight: bold;">{</span> :src =\> url<span style="color: #006600; font-weight: bold;">(</span>:report, 'a unique identifier'<span style="color: #006600; font-weight: bold;">)</span> <span style="color: #006600; font-weight: bold;">}</span>

</div>

</div>

I know, I know. What's with the 'a unique identifier' crap? Well, technically, the Reports\#show method should be showing some single unique entity, identified by the passed parameter (params\[:id\]). However, I wanted to keep things simple, for myself as much as for those reading this. As your application gets more complicated, you may want to add more reports. I am sure I will eventually, even if it's just 'weight change over past month', 'weight change over past 2 months', and 'weight change over past 3 months'. At that point I will identify what reports I wish to use by the passed id. Until then, this is working as a quick, dirty solution.

For now, I've got a very simple report. It looks like this:

<div class="centerize">

![Screenshot of Weight Report](http://soyunperdedor.com/files/sparklines_weight_tracker.png "I know, I'm ugly!  I was not designed by someone who knows anything about design!")

</div>

That's the very basic way to pull off Sparkline reports in Merb. Again, you can have several unique graphs easily enough. You could also break the basic options for 'plot\_to\_image' into another method, as did Mr. Topfunky. Caching the results of these graphs is probably not a bad idea either. These things are all up to you, for now. Enjoy playing with your graphs. May they be as depressing and tear-provoking for you as they have been for me.

**Note:** The weights in that screen shot do not represent real data. I haven't weighed so little since I was dirt poor and living off celery and peanut butter for meals once or twice a day. (No really, that happened. Hungry constantly, but that's the best I've every looked. HA\!) Also, I know what Mr. Topfunky's name is. It's a lot more entertaining to me to refer to him as Mr. Topfunky. It's no [Mr. Splashy Pants](http://www.greenpeace.org/international/campaigns/oceans/whaling/great-whale-trail/mrsplashypants), but it's close.
