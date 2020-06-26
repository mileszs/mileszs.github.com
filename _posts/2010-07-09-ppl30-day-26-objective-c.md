---
layout: post
title: "PPL30 Day 26: Objective-C"
date: 2010-07-09 23:44:43 -0400
comments: true
categories: [ppl30, programming]
---
<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

## Installation ##

Use `gcc`, or XCode, and, from what I understand, be on a Mac, or despair.

<em>At this point, I [searched Google images for 'apple fanboy'](http://images.google.com/images?hl=en&tbs=isch%3A1&sa=1&q=apple+fanboy&aq=f&aqi=g1&aql=&oq=&gs_rfai=). Do not do this. Do not click on image #2.</em>

## Resources ##

I did a lot of Googling, again. It didn't work very well, this time. I should have scheduled a session with [CZ](http://twitter.com/netshade). That would've been much more productive. **Warning:** I couldn't get anything to compile, which could be a testament to my ineptitude as much as it could be deficiencies in the following resources. `gcc` at your own risk (of frustration).

### Objective-C Beginner's Guide ###

Initially, I thought it was a bit suspect that all of the examples in the [Objective-C Beginner's Guide](http://www.otierney.net/objective-c.html) were, used with permission, from Sams Publishing. I always associated Sams with "Learn Some Junk in 21 Days!" books, which I never found quite as helpful as the title would imply.

"Unfair," I mused. "They surely put out books of higher quality to which I've simply not been exposed.

Then, though I learned something from the guide, I couldn't get any examples to compile. My suspicions and my frustration combined to thwart reason, which lead to my punching the coffee table. Now my hand hurts, thus typing this article hurts. I blame Sams.

### OOP in Objective-C Note from IU ###

This is brief, but I thought it worth mentioning: The [notes on compiling Obj-C from IU](https://www.cs.indiana.edu/classes/c304/ObjCompile.html) were helpful in getting 2 of the 3 steps resolved to compiling the first interface/implementation example from [Objective-C Beginner's Guide](http://www.otierney.net/objective-c.html). However, the List.h, List.m, and main.m files that [were provided on the Intro page](https://www.cs.indiana.edu/classes/c304/oop-intro.html) don't seem to play well together, or with `gcc -lobjc`. 

## What I Think I Now Know ##

Objective-C seems to be a pretty interesting take on OOP using C. I was surprised at how much it does not look like or feel like C. I wish I could have learned (and written) more Objective-C, and spent much less time fighting tools and the like. I feel very inept.

One interesting concept to me is that, in Objective-C, to define a class takes two files (or perhaps that's simply a convention): an interface definition and an implementation. The best analogy I can imagine is it's like writing a detailed, structured outline of the specific requirements of your new house (down to the type and color of carpet for every room) before any attempt at the blueprint. In that scenario, it makes perfect sense. If you're just building a shack, though, it seems like a waste of time, especially if you've built more than a few shacks in your life. Is this common elsewhere? (C++, maybe? I remember nothing of C++ other than `cout << "Hello World!"`.)

![shaq statue](/images/shaq-statue.jpg "When I said 'build a shack', you thought 4x scale 7 foot 2 inch American basketball player Shaquille O'neal?!") 

I like to make a big deal of the brackets in Objective-C for comedic effect (which is not really effective any longer -- everyone has built up a tolerance to lame bracket jokes, I think). They're not really that bad, especially having used, for instance, Lisp. (Parens have a different purpose in Lisp than in Objective-C, though, in that there is a purpose. _HO HO! I'm not done yet, folks._ Don't close that browser!) Honestly, though, forget about syntax complaints. It's always a red herring.

Well, unfortunately, that's it for today's episode. My hand hurts, I have a headache, I think I contracted a cold. Thankfully, everyone is blaming everything unfairly on a guy named Lebron James these days, so instead of being angry at something deserving of my contempt, I'll blame him, like the rest of the lunatics. That jerk.

* * *

Join us next time when our hero attempts to complete the Emacs tutorial without losing the friendships of his Vim brethren!
