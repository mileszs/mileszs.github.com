---
layout: post
title: "PPL30 Day 9: Groovy"
date: 2010-06-22 16:50:08 -0400
comments: true
categories: [ppl30, programming]
---
I'm jamming with Groovy today, a dynamic language built on the JVM. Groovy claims to be inspired by Ruby, Python, and Smalltalk, so this should be interesting.

<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

![Jimi Hendrix pic](/images/jimi-groovy.jpg "It's groovy, man. Right?")

## Installation ##

    brew install groovy

## Resources ##

### The Groovy Website ###

For reasons unknown, I started with [the Groovy website](http://groovy.codehaus.org/), and I never left. I'm not sure it's the best place to start, or even the best place to end, but it held my attention, so I suppose it gets a +1 for that.

The Beginner's Tutorial section is odd. It starts with a 'Getting Started' section, then 'Code as Data'. So far, so good. The 'Classes and Objects' section is lacking in substance. The following two sections deal with regular expressions! Then, Groovy SQL, which starts off with a note advising you to not worry if something doesn't make sense. Okay! No worries, then!

## What I Think I Now Know ##

Groovy, syntactically, reminds me of Javascript. Even camel casing is the norm (versus underscores). I'd consider the comparison to Javascript somewhat a compliment. At the least, I expected worse!

One interesting tidbit for Rubyists that I noticed is the similarity of block syntax:


<% coderay(:lang => :ruby, :line_numbers => :inline) do #_ -%>
  # Ruby
  (1..100).each { |num|
    # stuff
  }
  # I know do..end is idiomatic
  # The curlies are to ease comparison to groovy
<% end -%>

I remember \_why, in [why's [poignant] Guid to Ruby](http://mislav.uniqpath.com/poignant-guide/), talking about the two pipe characters acting as a chute, guiding the name to be used in the block below.


<% coderay(:lang => :groovy, :line_numbers => :inline) do #_ -%>
  // Groovy
  (1..100).each { num ->
    // stuff
  }
<% end -%>

\_why's explanation works for Groovy as well, except, in this case, we've an arrow that points the way to the block.

So, yes, Groovy has closures. Ruby-like block syntax seems to be the norm, especially when it somes to iterating over a list or map.

Groovy has several methods for operating on lists, as you might imagine. For instance, the `each()` I use in the code below. In addition, they have the very cool method '**star-dot**'. 

In Ruby, we might write:

<% coderay(:lang => :ruby, :line_numbers => :inline) do #_ -%>
  [1,2.3].collect {|num| num * 2 }
<% end %> 

In Groovy we'll write:

<% coderay(:lang => :groovy, :line_numbers => :inline) do #_ -%>
  [1,2,3]*.multiply(2)
<% end %>


Cool, huh? Star-dot, or `*.` is essentially shortcut syntax for collect.

Let's get to some Groovy FizzBuzz:

<% coderay(:lang => :groovy, :line_numbers => :inline) do #_ -%>
  class FizzBuzz {
    def result

    FizzBuzz() { ; }

    def perform() {
      (1..100).each { num ->
        result = '';
        if (num % 3 == 0) {
          result = result + "Fizz";
        }
        if (num % 5 == 0) {
          result = result + "Buzz";
        }
        if (result == '') {
          result = num;
        }
        println result
      }
    }
  }

  new FizzBuzz().perform();
<% end -%>

Not too interesting. I threw it inside of a class to learn something, and because I heard the JVM will kill you if you don't try your hardest to use **OOP** all the time.

A lot of Groovy really does feel like Javascript. It almost made learning anything about Groovy _more_ difficult. (Exhaustion didn't help.) The languages are so similar, wading through the syntax stuff that I felt like I "already knew" was tedious. I don't find Groovy nearly as fun as Scala or Clojure, but I would definitely rather use Groovy than Java! I hear, if you are an experienced Java developer, picking up Groovy is pretty easy. [Are You Experienced?](http://www.youtube.com/watch?v=UjyXsIYAp8o)

* * * 

And so it goes. Tomorrow? **Erlang.** I haven't touched Erlang at all -- I'm not sure I've ever seen any Erlang. It ought to be a great adventure!
