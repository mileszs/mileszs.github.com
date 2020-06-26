---
layout: post
title: "PPL30 Day 13: DataMapper"
date: 2010-06-26 23:15:39 -0400
comments: true
categories: [ppl30, programming]
---
I have been getting down with DataMapper in a Rails 3 app. Compared to the past week-and-five-sevenths, this is a homecoming of sorts. DataMapper is an ORM that sort of grew up with Merb. Now that Merb ate Rails, DataMapper is all growed up and 1.0 and stuff. I'm so proud!

<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

## Installation ##

The following is in my Gemfile (or you can install these separately):

    gem 'dm-rails',               DM_VERSION
    gem 'dm-postgres-adapter',  DM_VERSION
    gem 'dm-migrations',        DM_VERSION
    gem 'dm-types',             DM_VERSION
    gem 'dm-validations',       DM_VERSION
    gem 'dm-constraints',       DM_VERSION
    gem 'dm-transactions',      DM_VERSION
    gem 'dm-aggregates',        DM_VERSION
    gem 'dm-timestamps',        DM_VERSION
    gem 'dm-observer',          DM_VERSION

Put that in your Gemfile. Then:

   bundle install

**Bam!**

## Resources ##

### READMEs on GitHub ###

The [dm-rails repo's README on GitHub](http://github.com/datamapper/dm-rails). It gives you a command to run that creates your Rails app from a preconfigured template. Consequently, some knowledge will go ungained due to abstraction, but I am trying to learn DataMapper, man, not Rails 3 configuration.  There's also [an app called datamapper_on_rails3](http://github.com/snusnu/datamapper_on_rails3) on GitHub that you can clone and use. It's really the same difference either way.

### DataMapper.org ###

I seem to remember [datamapper.org](http://datamapper.org) having always been awesome, including its Merb-fueld early days. That hasn't changed. I found it to be the best place to learn about DataMapper (with both explanations and code samples).

## What I Think I Now Know ##

**Listen:** Let's be honest with each other; DataMapper and ActiveRecord are not _that_ different. Really. If you're going to choose one over the other, you are probably doing so based on a minor difference. The most prominent differences are 1) properties/attributes/fields are defined in a migration in ActiveRecord, but in the actual model with the `property` method in DataMapper and 2) the syntax for adding conditions to a query almost never requires dropping to SQL.

I started rewriting [vimtweets.com](http://vimtweets.com) as a Rails 3 app to play with DataMapper a bit. Let's see an example from that to illustrate how attributes are defined in the model (and showcase a beneficial consequence of the feature):

<% coderay(:lang => :ruby, :line_numbers => :inline) do #_ -%>
  class Tip
    include DataMapper::Resource

    property :id, Serial
    property :body, String, :required => true, :length => (1..140)
    property :tweeted_at, DateTime
  end
<% end -%>

It's that simple. It's nice to see the properties in the model with using `annotate`. The added benefit? You can apply validation rules, specifically limit access, etc., on the property's declaration itself. I think makes models much cleaner, and it makes it easier to find out everything you need to know about a property. "Ah, this property is a `String`, requires less than 140 characters, and cannot be blank. Good." I'm not searching around in the model and `db/migration/201080085585774885878738485_added_some_stuff_lol.rb`.

The conditions syntax, referred to as 'the hash syntax' in the DM docs, looks like this:

<% coderay(:lang => :ruby, :line_numbers => :inline) do #_ -%>
  class Tip
    include DataMapper::Resource

    property :id, Serial
    property :body, String, :required => true, :length => (1..140)
    property :tweeted_at, DateTime

    def self.untweeted
      all(:tweeted_at.not => nil)
    end
  end
<% end -%>

Yup. You basically add a method call to the key. It's awesome. The possible operators include `gt` `lt` `gte` `lte` `not equal` `equal` `like` `in` -- and, yes, you're expected to pass an array to 'in' as the value in the hash pair.

I love the idea of using this syntax over AR's syntax. However, DataMapper does support AR's syntax of `:conditions => {}` or `:conditions => ['']`, if you prefer that method.

I also love the idea of combining queries using set operators. Look at this example copied from [datamapper.org](http://datamapper.org):

<% coderay(:lang => :ruby, :line_numbers => :inline) do #_ -%>
  # Find all Zoos in Illinois, or those with five or more tigers
  Zoo.all(:state => 'IL') + Zoo.all(:tiger_count.gte => 5)
  # in SQL => SELECT * FROM "zoos" WHERE ("state" = 'IL' OR "tiger_count" >= 5)

  # It also works with the union operator
  Zoo.all(:state => 'IL') | Zoo.all(:tiger_count.gte => 5)
  # in SQL => SELECT * FROM "zoos" WHERE ("state" = 'IL' OR "tiger_count" >= 5)

  # Intersection produces an AND query
  Zoo.all(:state => 'IL') & Zoo.all(:tiger_count.gte => 5)
  # in SQL => SELECT * FROM "zoos" WHERE ("state" = 'IL' AND "tiger_count" >= 5)

  # Subtraction produces a NOT query
  Zoo.all(:state => 'IL') - Zoo.all(:tiger_count.gte => 5)
  # in SQL => SELECT * FROM "zoos" WHERE ("state" = 'IL' AND NOT("tiger_count" >= 5))
<% end -%>

DataMapper has a ton of other features, of course, including most of the every-day needs of an ActiveRecord user -- chained associations, single table inheritnance, etc. It also has some different features, some of which I mentioned above, and more, such as built-in Paranoia property types, or fairly trivial attachment to multiple data stores.

I really like DataMapper. I'm going to continue to use it to build a Rails 3 version of vimtweets.com. I'll need to spend some significant amount of time using it before I can say that I think it's actually a viable replacement for ActiveRecord. At first glance I would say it very well could be. With the property definitions and hash syntax for finding records, I think it could be worth it, as well.

* * *
Tomorrow I'm going to look into HTML5. I was inspired by [VexTab](http://vexflow.com/tabdiv/tutorial.html), a guitar tab editor in HTML5. Check out [Music Notation with HTML5 Canvas](http://0xfe.blogspot.com/2010/05/music-notation-with-html5-canvas.html)!
