---
layout: post
title: "PPL30 Day 23: Varnish"
date: 2010-07-06 23:21:08 -0400
comments: true
categories: [ppl30, programming]
---
Today I'm tackling Varnish, an HTTP accelerator -- or, more appropriately, a caching reverse proxy. Varnish is [used by such large sites](http://ingvar.blog.linpro.no/2010/01/21/the-usage-of-varnish-revisited-2/) as Hulu, Twitter, Photobucket, and many, many more. It is a powerful piece of software.

<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

![varnish](/images/varnish.jpg "Not this kind of Varnish.")

## Installation ##

    brew install varnish

I expected nothing less!

## Resources ##

## Graceful Caching with Varnish ##

[Graceful Cachin with Varnish](http://overstimulate.com/articles/varnish-getting-started) is a good place to get started with Varnish. It offers a fairly straightforward guide to getting started quickly playing with Varnish on your local machine. I definitely recommend starting here if you're new to Varnish.

### Varnish: It's not just for wood anymore ###

[This article](http://www.engineyard.com/blog/2010/varnish-its-not-just-for-wood-anymore/) is a in depth overview of Varnish and how to use it. It was written by an EngineYard employee (Kirk Hanines). You can be sure that it is well-written, and has extensive coverage of Varnish. It's a good place to learn more

### Varnish Website ###

Of course, we can't forget about [the Varnish website](http://varnish-cache.org/), and, in particular, [the Varnish introduction](http://varnish-cache.org/wiki/Introduction).

## What I Think I Now Know ##

Basically, Varnish sits between the front-end HTTP server and your app, caching stuff by watching the HTTP headers. It is used by default on Heroku. In fact you can take advantage of it simply by adding headers to your response. From [the Heroku docs](http://docs.heroku.com/http-caching):

<% coderay(:lang => :ruby, :line_numbers => :inline) do -%>
class MyController < ApplicationController
  def index
    response.headers['Cache-Control'] = 'public, max-age=300'
    render :text => "Rendered at #{Time.now}"
  end
end
<% end %>

So, I thought, why not see what happens when we do that to an app that is currently performing quite poorly? Well, there's probably something else really, really wrong causing such ridiculously awful performance, or my testing method is poor, or both. I didn't see an improvement (from the benchmark numbers) from enabling the cache, though I believe there was a difference when accessing via the browser. It will take more work on other areas of this app before I provide any sort of benchmark.

![failed](/images/mcfail.jpg "I failed. Seeing other people failing makes me feel better.")

It's definitely worth following along with [Graceful Cachin with Varnish](http://overstimulate.com/articles/varnish-getting-started), to get familiar with some of Varnish's settings, and the DSL for configuring Varnish, often called `VCL`. Once you get a feel for it, start looking at [EngineYard's article](http://www.engineyard.com/blog/2010/varnish-its-not-just-for-wood-anymore/) -- where things get very granular.

Varnish is an interesting piece of softare. (However, I've not had any experience with any other software that fulfills the same purpose, such as Squid.) I think it's interesting that VCL looks Perl-ish, is compiled to C source code, and then is compiled to a shared object, all on the fly. This seems insane to me, but it does mean a lot of options for configuring and tuning Varnish.

This is another very good tool to have in the arsenal when battling performance issues in web apps. I recommend giving the above sites a quick skim. Performance and caching are two areas in which I definitely lack knowledge, and I don't think I'm alone. You only really need to know it when you really need to know it, but then it's too late, right?
