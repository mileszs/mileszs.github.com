---
layout: post
title: "PPL30 Day 8: Clojure"
date: 2010-06-21 22:29:04 -0400
comments: true
categories: [ppl30, programming]
---
<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

We're really getting into the fun stuff on this trip, now. Don't get me wrong: databases were fun, other Ruby implementations were a practical diversion, but it's other languages that really interest me. I've been wanting to learn a Lisp dialect more deeply than just some academic reading. Though I wondered if it was more appropriate to start with Scheme, another language on my PPL30 list, I couldn't resist jumping to Clojure. (Yeah, I succumbed to the hype.)

## Installation ##

    brew install clojure
    brew install clojure-contrib

I threw in the extra line, though not necessary, as a sort of decoration. Think of it as the house plant to this post's suburban starter house.

## Resources ##

![Home Alone pic](/images/home-along.png "He's reacting to the sheer size of everything related to Clojure")

### Clojure Website ###

[The Clojure site](http://clojure.org/) is a nice site with _plenty_ of material, most of which I did not even come close to having time to see. That includes [some screencasts](http://clojure.blip.tv/)! 

### Clojure for Ruby Programmers ###

[Clojure for Ruby Programmers](http://rubyconf2009.confreaks.com/21-nov-2009-10-25-clojure-for-ruby-programmers-stuart-halloway.html) is a talk given at RubyConf 2009 by Stuart Halloway. It's still a bit of a blur to me, as Stuart moves quickly and packs in a lot of information. An audience member describes it as drinking KoolAid from a firehose near the end.

### Clojure - Functional Programming for the JVM ###

From Object Computing, Inc is an 'introduction' to Clojure, [Clojure - Functional Programming for the JVM](http://java.ociweb.com/mark/clojure/article.html). This was my primary resource for today's session. I actually found it a lot easier to skim than I thought it was going to be. However, it is really long, and stocked full of a ridiculous amount of information. I haven't reached the end!

## What I Think I Now Know ##

As I mentioned, Clojure is a Lisp dialect. Consequently, some terms are used a bit differently, the style is quite _functional_, prefix notation is used (instead of the typicaly infix notation), 'special forms', 'predicates', 'quoting', etc. Stuff that makes perfect sense, but can trip you up if you're not concentrating.

There are a lot of interesting concepts that are introduced as well. For instance, the obvious and famous: data and code have the same representation. This is a function call: `(a b c)` (it calls 'a' with parameters 'b' and 'c'). This is a list: `'(a b c)`. Any exceptions to this rule is considered syntactic sugar. It's up for debate as to what is too much, or what is too little, syntactic sugar. That is much of what makes one Lisp different from another.

Clojure FizzBuzz!:

<% coderay(:lang => :clojure, :line_numbers => :inline) do #_ -%>
  (doseq [num (range 1 100)]
    (println
      (cond
        (= 0 (mod num 15)) "FizzBuzz"
        (= 0 (mod num 3)) "Fizz"
        (= 0 (mod num 5)) "Buzz"
        true num)))
<% end -%>

It's kinda pretty, isn't it? I'll try to explain: `doseq` iterates over a collection (in this case, the range 1 to 100, assigned to 'num'). We're simply printing the result of the conditional expression. `cond` evaluates each statement in its body in order, returning the result of the first one whose result is 'true'. Done!

It's hard for me to say whether I've come out of today's session liking Lisp, or liking Clojure, or both. I definitely like something. I'm really glad that I now feel like I have _some_ clue about Lisp. I look forward to learning Scheme, as well as using this basic knowledge to dive a bit deeper into Lisp even after PPL30 is complete!

![Indiana Jones Tweed Coat pic](/images/IndyClassroom3.jpg "This is what Lisp lovers all look like")

* * *

Today, Tim covered [horizontal formatting in code](http://timharvey.net/2010/06/21/ppl-day-8-horizontal-formatting/) in his Clean Code reading. I have to admit, I'm with the book on this one. I'm less OCD than I thought!

I'm going to stick with the JVM, for better or for worse. It's really tempting to jump straight to Scheme for tomorrow, but instead, I'll hit Groovy. (I think I'm going to order The Little Schemer, and put of Scheme until it gets here!)
