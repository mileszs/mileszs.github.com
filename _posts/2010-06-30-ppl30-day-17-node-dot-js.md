---
layout: post
title: "PPL30 Day 17: Node.js"
date: 2010-06-30 23:12:12 -0400
comments: true
categories: [ppl30, programming]
---
This is quite an event! ([All puns absolutely intended.](http://thatspunny.blogspot.com/)) I've been studying Node.js -- **"Evented I/O for V8 JavaScript."** In other words, put JS on the server and allow it to use its natural faculties for evented programming. That's the idea, anyway. 

![joe whoa](/images/joey-whoa.gif "Whoa! An abstraction of evented programming in a language in which I already know how to do evented programming?! WHOA!")

<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

## Installation ##

    brew install node

And we're back!

## Resources ##

### Video by Ryan Dahl ###

The [presentation given at JSConf Berlin](http://jsconf.eu/2009/video_nodejs_by_ryan_dahl.html) (held November 7th and 8th, 2009), given by its creator Ryan Dahl is well worth watching. Mr. Dahl seems a bit nervous through most of it. However, he makes a great case for Node's existence, which I know is the primary concern many people have. I mean, why Javascript on the backend? Why evented and not threads? He then proceeds to give some examples of how one might use Node (TCP server, HTTP server, IRC server...).

### Node is Genuinely Exciting ###

A gentleman named Simon Willison wrote [a good introduction](http://simonwillison.net/2009/Nov/23/node/) towards the end of November last year. It has many pieces from Mr. Dahl's video, mentioned above, but you can read it in less than 48 minutes. Do note, however, that the API for Node has changed, so some of those code examples will not work. Thankfully, Node warned me that, for instance, _"sendBody is now called write()"_, or something to that effect.

### Documentation ###

This sort of goes without saying, typically; use the docs. However, the nice thing about [the Node.js docs](http://nodejs.org/api.html) is that they are pretty usable, and pretty readable. Check them out.

## What I Think I Now Know ##

### Why Node? ###

Currently, web servers (as one example) are typically written using OS threads. Threads have significant amount of overhead. Event-driven servers, such as Nginx tend to be faster, and consume less memory. Great! Let's all do stuff evented style. Right? Amirite? Sure! Except that's hard. Most programming languages lack an easy or natural way to do event-driven things. Even libraries such as EventMachine and Twisted have to fight their languages (Ruby and Python, respectively) a bit. 

I don't know if you've noticed, but Javascript was sorta built to be driven by events. Javascript and events are tight, man. They're close. Sonny &amp; Cher. Coffee &amp; cream. Nick Nolte and rehab.

![Nick Nolte mug](/images/nolte-mug.jpg "Java-what? I was already at an event. Where am I? Can I have a beer and a hooker?")

### Okay. What does Node do? ###

Node runs on Google's V8 (written in C). Consequently, it's pretty fast. Check out Mr. Willison's article above for some really basic tests. You can also probably find a benchmark or two in the tubes.

Node was created as a framework for easily creating evented servers, in a language that lends itself well the art of evented programming. From what I can tell, it does this pretty well!

Let's look at my FizzBuzz, with a spin this time:

<% coderay(:lang => :javascript, :line_numbers => :inline) do #_ -%>
function fizzbuzz(num) {
  var result = '';
  if (num % 3 == 0) { result = 'Fizz'; }
  if (num % 5 == 0) { result = result + 'Buzz'; }
  if (result == '') { result = num; }
  return result;
}

var sys = require('sys'),
  http = require('http'),
  url = require('url');

http.createServer(function(req, res) {
  var query = url.parse(req.url, true).query;
  var msg = '';
  if (query && query.num) {
    msg = fizzbuzz(query.num);
  } else {
    msg = 'Is this some kinda joke?';
  }
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end(msg + "!\n");
}).listen(8080);
sys.puts('Server running at http://localhost:8080');
<% end -%>

That is a basic FizzBuzz function, with a quick HTTP server glued on. If you've installed node, you can copy and paste that code to `fizzbuzz.js`, and then run `node fizzbuzz.js`. In another terimanl window, try `curl localhost:8080?num=48`. You should see **Fizz!** returned.

I'll throw a couple wrinkles in here -- wrinkles you'll see in nearly every Node.js HTTP server example, but, nonetheless:

<% coderay(:lang => :javascript, :line_numbers => :inline) do #_ -%>
function fizzbuzz(num) {
  var result = '';
  if (num % 3 == 0) { result = 'Fizz'; }
  if (num % 5 == 0) { result = result + 'Buzz'; }
  if (result == '') { result = num; }
  return result;
}

var sys = require('sys'),
  http = require('http'),
  url = require('url');

http.createServer(function(req, res) {
  var query = url.parse(req.url, true).query;
  var msg = '';
  if (query && query.num) {
    msg = fizzbuzz(query.num);
  } else {
    msg = 'Is this some kinda joke?';
  }
  res.writeHead(200, {'Content-Type': 'text/plain'});
  setTimeout(function() {
    res.end(msg + "!\n");
  }, 2000);
}).listen(8080);
sys.puts('Server running at http://localhost:8080');
<% end -%>

That one will wait 2 seconds before sending the response.



<% coderay(:lang => :javascript, :line_numbers => :inline) do #_ -%>
function fizzbuzz(num) {
  var result = '';
  if (num % 3 == 0) { result = 'Fizz'; }
  if (num % 5 == 0) { result = result + 'Buzz'; }
  if (result == '') { result = num; }
  return result;
}

var sys = require('sys'),
  http = require('http'),
  url = require('url');

http.createServer(function(req, res) {
  var query = url.parse(req.url, true).query;
  var msg = '';
  if (query && query.num) {
    msg = fizzbuzz(query.num);
  } else {
    msg = 'Is this some kinda joke?';
  }
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.write("Before FizzBuzz.\n");
  setTimeout(function() {
    res.end(msg + "!\n");
  }, 2000);
  res.write("Also before FizzBuzz.\n");
}).listen(8080);
sys.puts('Server running at http://localhost:8080');
<% end -%>

The output for this one?

    curl localhost:8080?num=48

    Before FizzBuzz.
    Also before FizzBuzz.

... 2 seconds ...

    Fizz!

Fun! It, of course, went straight to executing the `write()` after the timeout while waiting for the timeout.

Check out [the projects built on node](http://wiki.github.com/ry/node/) so far. That list is a bit incomplete, though. I'm anxious to try the Sinatra-inspired [Express web development framework](http://github.com/visionmedia/express) written in Node. Also, [the API docs](http://nodejs.org/api.html) are quite readable. You'll learn a lot if you just read through them. Overall, it's a really interesting project, and you probably already know Javascript, so it shouldn't be too difficult to pick up!

* * *

This article provided me a nice segue to Nginx, didn't it? That's what's up next. Now [go see Tim](http://timharvey.net/) and congratulate him on doubling up a day this week!
