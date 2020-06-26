---
layout: post
title: "PPL30 Day 19: Unicorn"
date: 2010-07-02 22:56:27 -0400
comments: true
categories: [ppl30, programming]
---
<a href="http://www.cornify.com" onclick="cornify_add();return false;">Unicorn</a>. This thing is called 'Unicorn'. I don't know how to start off a post about a project named 'Unicorn'.

[Unicorn](http://unicorn.bogomips.org/) is an HTTP server for Ruby, similar to Mongrel or Thin. However, the twist is that Unicorn has its own master that manages the workers, and leaves balancing up to the OS. In this way, it's got a bit more in common with Passenger, actually. (Technical details: The workers `select()` on the socket, only serving requests they're capable of handling -- hence the kernel doing the balancing. This only sort of makes sense conceptually for me. I don't think there's any reason to feel bad if you just fell asleep reading it.)

<a href="http://www.cornify.com" onclick="cornify_add();return false;">
![unicorn](/images/unicorn.gif "I love this unicorn. I can hold pizza bagels on its horn!")
</a>

<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

## Resources ##

The resources used comprise the meat of this particular article. There's simply not much to tell born of my experiences with <a href="http://www.cornify.com" onclick="cornify_add();return false;">Unicorn</a> tonight. Technically, there's not much for me to say that isn't covered better elsewhere.

### GitHub Loves Unicorns ###

In October of 2009 (I can hardly believe Unicorn has even been around that long), Chris Wanstrath wrote [an article about GitHub's use of Unicorn](http://github.com/blog/517-unicorn) on the GitHub blog. As far as I could tell, it's actually one of the better guides to Nginx and <a href="http://www.cornify.com" onclick="cornify_add();return false;">Unicorn</a>. 

### Unicorn's Docs ###

[The official Unicorn site](http://unicorn.bogomips.org/) is pretty good, though not the clearest site to navigate. In particular, I recommending checking out [the Unicorn configurator page](http://unicorn.bogomips.org/Unicorn/Configurator.html), which links to a few example config files. It's probably worth a healthy skim or two, as well.

### Tomayko's 'Unicorn is Unix' ###

Ryan Tomayko [wrote an article](http://tomayko.com/writings/unicorn-is-unix) detailing Unicorn's use of Unix-y things. It's highly technical, geeky, and good fun.

## What I Think I Now Know ##


### Benchmarks of Questionable Validity ###

As I switched Apache for Nginx last night, I switched Passenger for Unicorn tonight. As I think it might be useful (though it's mostly copied straight from the Unicorn example), this is my Nginx config with all of the commented sections removed:

<% coderay(:lang => :ruby, :line_numbers => :inline) do #_ -%>
worker_processes  1;

events {
    worker_connections  1024;
}

http {

    include       mime.types;
    default_type  application/octet-stream;
    sendfile        on;

    keepalive_timeout  65;

    upstream app_server {
      server unix:/var/www/signal/tmp/sockets/unicorn.sock fail_timeout=0;
    }

  server {
    listen 80 default;

    client_max_body_size 4G;
    server_name scruffy;

    keepalive_timeout 5;

    root /var/www/signal/public;

    location / {
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header Host $http_host;
      proxy_redirect off;
      if (!-f $request_filename) {
        proxy_pass http://app_server;
        break;
      }
    }
  }
}
<% end %>

The following is my unicorn config:
<% coderay(:lang => :ruby, :line_numbers => :inline) do #__ -%>
rails_env = ENV['RAILS_ENV'] || 'production'
worker_processes (rails_env == 'production' ? 4 : 2)
preload_app true
timeout 30
listen '/var/www/signal/tmp/sockets/unicorn.sock'
<% end %>

To start <a href="http://www.cornify.com" onclick="cornify_add();return false;">Unicorn</a>, I ran:

    unicorn_rails -c /var/www/signal/config/unicorn -E production -D

### Benchmarks of Questionable Validity ###

    # Command
    httperf --server scruffy --uri / --num-conns 200

    # unicorn
    Reply rate [replies/s]: min 7.2 avg 7.6 max 7.8 stddev 0.3 (5 samples)

    # passenger
    Reply rate [replies/s]: min 7.6 avg 7.7 max 7.8 stddev 0.1 (5 samples)

At this point, I shrugged. I don't think my little CI server is a great example, nor do I think my 'benchmarks' qualify as anything on which anyone should hang a hat. I wouldn't know that the data is uninteresting if I didn't try it, though. There are plenty of people willing to [test performance of servers](http://labs.revelationglobal.com/2009/10/06/mongrel_passenger_unicorn.html) more stringently than myself.

That said, I do think <a href="http://www.cornify.com" onclick="cornify_add();return false;">Unicorn</a>.

* * *

MooTools tomorrow! I heard it combined jQuery's DOM interaction with Prototype's class stuff. In theory? Awesome! In practice? Man, I hope so.

<script type="text/javascript" src="http://www.cornify.com/js/cornify.js"></script>
