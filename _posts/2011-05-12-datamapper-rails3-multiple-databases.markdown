---
layout: post
title: "Datamapper for Multiple Databases in Rails 3"
date: 2011-05-12 5:59
categories: programming
---

[DataMapper]("http://datamapper.org") is awesome. It’s flexible, you
define the properties in the model itself, it deals with all sorts of
columns, it can create UUIDs easily, it is fantastic for dealing with
*legacy*, or just *not designed with Rails’ ActiveRecord in mind*
databases. Hell, it shoveled my driveway, hooked up my cable, and
delivered our beautiful, 7 pound, 2 ounce baby boy last Tuesday. (I kid!
I don’t have a baby. **Yet**.)

It can also connect to multiple databases, even when used as the ORM in
a Rails 3 app. Unfortunately, this isn’t mentioned on [DataMapper’s
website]("http://datamapper.org"), that I could find. However, people
with their wits about them (I am not in that set of people) might think
to check out [the README for the dm-rails project on
Github]("https://github.com/datamapper/dm-rails"). In there, it shows
how to lay out your `database.yml` properly. It’s important. Go see it.
Go. I’ll wait.

I mention this, and forced you to go look, mostly to help someone
searching frantically. You there. Guy who has been searching for this
frantically in futile frustration for an hour: Your journey is over.
Let’s move on.

I had to do a bit of work to get RSpec and Cucumber behaving correctly.
I want my test databases, or *repositories*, cleared after each test. I
solved RSpec first, so we’ll start with that. My `spec/spec_helper.rb`
looks something like this:


``` ruby
ENV["RAILS_ENV"] ||= 'test'
require File.expand_path("../../config/environment", __FILE__)
require 'rspec/rails'

Dir[Rails.root.join("spec/support/**/*.rb")].each {|f| require f}

RSpec.configure do |config|
  config.mock_with :rspec

  config.before(:each) do
    DataMapper::Model.descendants.each do |model|
      DataMapper.repository(model.default_repository_name).adapter.execute("SET foreign_key_checks = 0")
      DataMapper.repository(model.default_repository_name).adapter.execute("TRUNCATE TABLE #{model.storage_name}")
    end
  end
end
```

Clearly the code above is specific to MySQL. If you’d rather avoid
shackling a specific database server to your app’s ankle, you might try
something like [database_cleaner]("https://github.com/bmabey/database_cleaner").

The interesting part is probably
`DataMapper.repository(model.default_repository_name)`. I define
`self.default_repository_name` differently for models that aren’t using,
well, `:default`. That allows this to work (among other things in
DataMapper). I recommend it. For instance:


``` ruby
class SecondaryUsinThing
  include DataMapper::Resource
  def self.default_repository_name
    :secondary
  end
  # ...
end
```

I needed Cucumber to do some of this crazy junk also, of course. The
dm-rails gem recommended a before and after block to add to a
`features/support/datamapper.rb`, but I had to add a bit to mine to make
it work.


``` ruby
Before do
  DataMapper::Model.descendants.each do |model|
    DataMapper.repository(model.default_repository_name).adapter.execute("SET foreign_key_checks = 0")
    DataMapper.repository(model.default_repository_name).adapter.execute("TRUNCATE TABLE #{model.storage_name}")
  end

  [:default, :secondary].each do |repository|
    DataMapper.send("repository", repository) do |repo|
      transaction = DataMapper::Transaction.new(repo)
      transaction.begin
      repo.adapter.push_transaction(transaction)
    end
  end
end

After do
  [:default, :secondary].each do |repository|
    DataMapper.send("repository", repository) do |repo|
      repo.adapter.pop_transaction.rollback rescue nil
    end
  end
end
```

Familiar, right? That should get your testing mojo rising. It’s
basically opening the door for DataMapper, so he can come in and repaint
your nursery while teaching your newborn sign language, or one of his
many other talents.
