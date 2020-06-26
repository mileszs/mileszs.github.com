---
layout: post
title: "PPL30 Day 7: Ruby 1.9.2"
date: 2010-06-20 23:54:12 -0400
comments: true
categories: [ppl30, programming]
---
<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

My second non-1.8.7-line Ruby in 2 days! Wow. Ruby 1.9.2 is the future of the primary 'Matz' line of Ruby. It's important, and gets more important with each of its releases. I've been getting increased requests to make [WickedPDF](http://github.com/mileszs/wicked_pdf) 1.9.x compatible, so I better start learning this line now! (Thank you to those pressuring me on that front, as well, by the way. I need it!)

## Resources ##

Honestly, I recommend Google. That said, [this RubyInside link roundup](http://www.rubyinside.com/23-useful-ruby-19-links-and-resources-1498.html) resulted in some good information, but not all links were golden. I followed as many links as I could, and read the content on the other side. I think I learned a substantial amount about 1.9.2. That said, I think the recommended method for learning 1.9.x is to grab a library of yours with some good tests, and try to make them all pass.

While I'm here, though: should the [Is It Ruby 1.9?](http://isitruby19.com/) site have some sort of 1.9.x FAQ? Or a compatiblity checklist? Some sort of guide? Give me a freakin' wiki. They've got the "Look we built a site to track gem/library compatibility because we care" thing going. Shouldn't they have taken it to the next level? It's not a huge step. "Look, we care so much about you getting your code updated, we've provided this lists of things that are very likely to cause you headaches, this list of things that catch a decent amount of people, and this list of things taht are rare, but still might trip you up at some point."


## What I Think I Now Know ##

Some interesting things about 1.9.x:

   * You can now define !, !=, !~ methods
   * RubyGems is a first class citizen -- no more calling require 'rubygems'
   * `{one: '1', two: '2'}` is valid Hash literal syntax (so is `{:one => '1', :two => '2'}`
   * You can add method parameters after required parameters (`def something(one, two = 2, three)`)
   * Regular expressions have gotten some extra features (such as named groups)
   * Insertion order is now preserved in Hashes
   * [Fibers](http://ruby-doc.org/core-1.9/classes/Fiber.html) has a lot of people stoked about the concurrency possibilities

That's far from an exhaustive list. It's mostly just things that have stuck with me through all of the things I've read today. 

Hey! Today, I got [vimtweets.com](http://vimtweets.com) code to run on 1.9.2! I even got the specs to not produce any errors! Of course, they also didn't produce any output. For now, I'm going to mark that up as a net positive.

I decided to run httperf locally against WEBrick for the system's Ruby 1.8.7 install, RVM's REE, and RVM's 1.9.2-head. I used the following command:

    httperf --hog --server localhost --port=3000 --uri / --num-conns 1000

I ran that five times per Ruby version, as a weak consistency check. I've heard that [1.9.x is much faster](http://www.rubychan.de/share/yarv_speedups.html) than 1.8.x. I am excited to see these results:


### 1.9.2 
    Reply rate [replies/s]: min 4.8 avg 5.8 max 6.2 stddev 0.4 (34 samples)
    Reply rate [replies/s]: min 5.2 avg 6.1 max 6.4 stddev 0.3 (32 samples)
    Reply rate [replies/s]: min 4.0 avg 5.4 max 6.4 stddev 0.6 (36 samples)
    Reply rate [replies/s]: min 4.8 avg 6.0 max 6.4 stddev 0.3 (33 samples)
    Reply rate [replies/s]: min 5.2 avg 6.0 max 6.4 stddev 0.3 (33 samples)

### 1.8.7 ###
    Reply rate [replies/s]: min 6.4 avg 7.2 max 7.6 stddev 0.3 (27 samples)
    Reply rate [replies/s]: min 6.0 avg 7.0 max 7.6 stddev 0.4 (28 samples)
    Reply rate [replies/s]: min 6.2 avg 7.3 max 7.6 stddev 0.3 (27 samples)
    Reply rate [replies/s]: min 6.0 avg 7.0 max 7.6 stddev 0.3 (28 samples)
    Reply rate [replies/s]: min 6.8 avg 7.4 max 7.6 stddev 0.2 (27 samples)

### REE ###
    Reply rate [replies/s]: min 8.0 avg 8.9 max 9.6 stddev 0.4 (22 samples)
    Reply rate [replies/s]: min 8.2 avg 8.9 max 9.2 stddev 0.3 (22 samples)
    Reply rate [replies/s]: min 7.8 avg 8.7 max 9.4 stddev 0.4 (23 samples)
    Reply rate [replies/s]: min 8.0 avg 8.7 max 9.6 stddev 0.5 (22 samples)
    Reply rate [replies/s]: min 8.2 avg 9.1 max 9.6 stddev 0.4 (22 samples)

Well, that's not what I had in mind. Of course, this isn't the most scientific of tests. That said, I thought 1.9.2 was supposed to be hella-faster than 1.8.7. Right? I suppose I wasn't really doing anything calculation-heavy. Perhaps that would have made a difference. As it stands, the home page of vimtweets is pretty straigthforward.

Anyway, 1.9.2 has some interesting features. Unfortunately, I have a hard time justifying the time it would take to make all of my apps 1.9.2 compatible. I'll do WickedPDF. Unless some scenario occurs in which I am yet again compelled to take a hard look at 1.9.2, I'm afraid I won't be spending much time with it. 

(Clearly this post is begging for someone to argue with me. I really, really want to love 1.9.2. Tell me what I'm missing!)

* * *

I'll jump back in to learning new languages tomorrow, starting with Clojure!

Check out [Tim's Day 7 post](http://timharvey.net/2010/06/20/ppl-day-7-vertical-formatting/) as well, covering vertical formatting from Clean Code. Happy Father's Day, Tim!
