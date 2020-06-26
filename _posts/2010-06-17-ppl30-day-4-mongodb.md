---
layout: post
title: "PPL 30 Day 4: MongoDB"
date: 2010-06-17 22:39:29 -0400
comments: true
categories: [ppl30, programming]
---
<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

MongoDB is a document-store-style database  in the NoSQL genre. You know, all of these NoSQL databases are starting to bleed together a bit. It's good this is the last one (for a while, anyway).

## Installation ##

    brew install mongodb

BAM!

## Resources ##

### MongoDB.org ###

Wow. You know what? The [actual MongoDB site](http://www.mongodb.org/) is excellent. It seems so rare, that I'm genuinely a bit stunned. Seriously, though, it's great. In particular, there's a "Try It Out" link that opens up a little console-like interactive div. Within that, you can start a short tutorial that gets you really working with MongoDB. Honestly, browsing the MongoDB site was really enjoyable.

## What I Think I Now Know ##

MongoDB is document-oriented, meaning that the units stored in it are typically collections of key-value pairs. The coolest part of MongoDB? It stores what is essentially JSON (though officially BSON -- Binary JSON). I think a code sample (from mongo's console) might be necessary to illustrate this:

    > doc = { author: 'Miles', title: 'Miles is Awesome'}
    = { "author" : "Miles", "title" : "Miles is Awesome" }
    > db.posts.insert(doc);
    > db.posts.find();
    = { "_id" : ObjectId("4c1ae1f29108557657fb8ee3"), "author" : "Miles", "title" : "Miles is Awesome" }

I really love that. The result, for me personally, is that it's really easy to understand what you're putting in and what you're getting out. JSON is so common in web development today. Because of that, I think this is easy to understand for most web developers. That is important.

MongoDB also has a very interesting, and I imagine incredibly useful, querying ability. The most succint way to show this is with another 'code' snippet, I think.

    > db.users.insert({ first: 'Miles', last: 'Sterrett'});
    > db.users.insert({ first: 'John', last: 'Smith' });
    > db.users.insert({ first: 'James', last: 'Smith' });
    > db.users.find();
    = { "_id" : ObjectId("4c1ae3899108557657fb8ee4"), "first" : "Miles", "last" : "Sterrett" }
    = { "_id" : ObjectId("4c1ae38d9108557657fb8ee5"), "first" : "John", "last" : "Smith" }
    = { "_id" : ObjectId("4c1ae38f9108557657fb8ee6"), "first" : "James", "last" : "Smith" }
    > db.users.find({last: "Smith"})
    = { "_id" : ObjectId("4c1ae38d9108557657fb8ee5"), "first" : "John", "last" : "Smith" }
    = { "_id" : ObjectId("4c1ae38f9108557657fb8ee6"), "first" : "James", "last" : "Smith" }
 
 So, that's pretty straightforward. However, check this out:

    > db.users.insert({ first: 'Miles', last: 'Sterrett', awesome_score: 5});
    > db.users.insert({ first: 'John', last: 'Smith', awesome_score: 3 });
    > db.users.insert({ first: 'James', last: 'Smith', awesome_score: 2 });
    > db.users.find({awesome_score: {$gte : 3}});
    { "_id" : ObjectId("4c1ae4b99108557657fb8ee7"), "first" : "Miles", "last" : "Sterrett", "awesome_score" : 5 }
    { "_id" : ObjectId("4c1ae4c99108557657fb8ee8"), "first" : "John", "last" : "Smith", "awesome_score" : 3 }

_Whaaaaaaat?!_ Yeah. That's pretty awesome. If you're curious, I recommend looking into [the Advanced Queries](http://www.mongodb.org/display/DOCS/Advanced+Queries) section.

You know what MongoDB doesn't do? "Auto-sharding". <strike>In other words, you can't just add a node seamlessly as you can with Riak or Cassandra.</strike> (This is wrong. It's not the same. I'm an idiot.) You know what's supposed to be released in July 2010 for MongoDB? [Auto-sharding](http://www.mongodb.org/display/DOCS/Sharding).

MongoDB is a very interesting entry into this space. It seems clearly intended to engage programmers, with its JSON documents, its query syntax, and its very programmer-friendly interface. It has more organization and structure than, say, Riak, which I think is both a blessing and a curse. MongoDB immediately comes to mind for web apps because it has so many facilities that effectively mimic traditional RDBMS systems. It's not an uncomfortable transition. That said, that can also be a disadvantage in a climate that seems to increasingly value simplicity, and systems that get the hell out of the way. For instance, Riak seems similarly programmer focused (I suppose you could argue ALL of these NoSQL options are), but does more to stay the hell out of the way. It's an interesting trade-off.

I've already decided I'd like to rewrite [vimtweets.com](http://vimtweets.com) to use a NoSQL-style database. (Why? Fun, and further education, mostly.) It'll actually be hard for me to decide whether that means MongoDB or Riak. Good show, Mongo. Good show.
