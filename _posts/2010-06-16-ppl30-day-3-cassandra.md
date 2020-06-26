---
layout: post
title: "PPL 30 Day 3: Cassandra"
date: 2010-06-16 22:02:12 -0400
comments: true
categories: [ppl30, programming]
---
<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

We've seen Redis, which is sort of Memcached with a fancy pocket knife and some RedBull. We've seen Riak, which (is badass and) mostly wants to be like Amazon Dynamo. Today, we'll see Cassandra, which wants to be both Dynamo and Google's BigTable. What could possibly go wrong?

## Installation ##

_What?!_ No `brew` command? Ugh. (I should've added "brew installation recipes" to the PPL30 list. It could've replaced Cassandra.)

I used the [Getting Started](http://wiki.apache.org/cassandra/GettingStarted) entry in the Cassandra wiki. It should be noted that I'm running on Snow Leopard. That means all I had to do, after [downloading the tarball](http://cassandra.apache.org/download/), was:

    tar xvzf apache-cassandra-0.6.2-bin.tar.gz
    cd apache-cassandra-0.6.2
    sudo bin/cassandra -f

Off and running. It's no `brew`, but it'll do. (Doobie doobie do.)

## Resources ##

### The Cassandra Wiki ###

Where better to get information than [the Cassandra wiki](http://wiki.apache.org/cassandra/FrontPage)? I jumped around from section to section in there, hence linking to the front page rather than a specific section of the wiki. Just look around.

I will say, instead of reading [A Description of the Cassandra Data Model](http://wiki.apache.org/cassandra/DataModel), skip to this next article:

### WTF is a SuperColumn? ###

By Arin Sarkissian, [WTF is a SuperColumn? An Intro into the Cassandra Data Model](http://arin.me/blog/wtf-is-a-supercolumn-cassandra-data-model) is an attempt to clear up Cassandra's data model, so that fewer people are confused, so that more people will use Cassandra. I think it comes close, anyway. It helped. It uses something along the lines of JSON to illustrate some of the data model concepts, which is nice.

### Up and Running with Cassandra

Evan Weaver wrote a good, ruby-tinged overview of Cassandra called [Up and Running with Cassandra](http://blog.evanweaver.com/articles/2009/07/06/up-and-running-with-cassandra/). I, of course, didn't come across it until the end of my time with Cassandra. I wish I had seen it first.

## What I Think I Now Know ##

The confusing thing about Cassandra, if you didn't already know, is the data model. That's why the wiki has a separate article to cover it. That's why someone from Digg wrote "WTF is a SuperColumn?". That's why I'm about to try to lay it out for you.

First of all, I think a lot of people are trying too hard to explain it. (This is probably a clear indication that I still don't get it.) They are using a lot of jargon that only makes the whole thing more difficult to understand. I'm going to try to explain in the way that I wish it were originally explained to me.

The two most basic types of data are Columns and SuperColumns. At this point, everyone will start talking about them being 'tuples' because they have timestamps. Forget about that for now. It's an implementation detail. 

A Column is a key-value pair. The value is a string (that's not quite true, but the rest is just detail). A SuperColumn is a key-value pair. The value is a list of columns.

Let's say you now want to group these Columns and SuperColumns in a meaningful way. In a relational database, you'd use a table, right? To do this in Cassandra, you want one of two types of ColumnFamily (Standard, and, of course, Super). A ColumnFamily is just a name for a group of rows. A row is a key with the value of a list of columns. In a standard ColumnFamily, the list of columns is a list of standard Columns. In a SuperColumnFamily, the list of columns is a list of SuperColumns.

Lastly, you can even group ColumnFamilies via Keyspaces. Keyspaces are a bit like like databases in a typical RDBMS, except it doesn't impose any relationship between the tables -- you're not going to run a JOIN over these things. That said, you might use a keyspace to hold all of the ColumnFamilies in one of your applications, the same way you might use a single database in PostgreSQL to hold all of the tables for your application.

That's it. Oh, there's more to Cassandra that is probably worth learning if you're going to use it. It's all just details, though. 

Honestly, I learned some other stuff -- such as the fact that Cassandra allows you to add nodes and it'll figure it out for you, like Riak, or that columns are always sorted in Cassandra, and the way they're sorted is configurable at startup -- but I think the data model is the important thing to figure out if you're going to try to figure out Cassandra.

As for what I _really_ think of Cassandra? It seems like a lot of complication for little benefit. You have to edit an XML config file and restart nodes to add a new ColumnFamily, which seems silly. Ultimately, I just can't see a good reason for using Cassandra over something with a simpler 'interface', other than, perhaps, performance. I easily could be wrong, and I hope you'll try to convince me. As it stands today, I'd pick up Riak first.
