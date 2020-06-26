--- 
layout: post
title: Rails 2.1 Time Zone Trouble
date: 2008-08-10 21:41:47 UTC
comments: true
categories: 
--- 
Making an app deal with multiple time zones is easy in Rails 2.1. Right? Just include a default time zone your environment.rb:

    Rails::Initializer.run do |config|
    # ...
      config.time_zone = "Eastern Time (US & Canada)"
    # ...
    end

Rails will store time stamps in UTC in the database. They will be converted to the default time zone when they are requested. I thought that sounded great\! Although, at work, we don't currently need to support any other time zone than Eastern, there are a few reasons we suspect we may need to do so sooner than later. We welcomed the time zone support in Rails 2.1. Unfortunately, it didn't work out all that well for us, and we don't know why.

Basically, Rails isn't converting the times correctly. System time on the server is correct, so there has to be something wrong withour Rails config(s). The best way to illustrate the problem is with irb:

    # A newly created object
    $>script/console
    >> t = Ticket.last
    >> t.created_at
    => Tue, 05 Aug 2008 13:24:56 EDT -04:00 # NOT correct -- it should be 9:24 EDT -04:00
    >> t.created_at_before_type_cast
    => Tue, 05 Aug 2008 13:24:56 +0000 # Correct -- UTC
    >> Time.now
    => Tue Aug 05 09:25:34 -0400 2008 # Correct
    >> Time.zone.now
    => Tue, 05 Aug 2008 09:26:11 EDT -04:00 # Correct

So, the only thing that is not correct is the conversion from UTC in the database to local time. It seems to think that local time and UTC should be the same, and only changes the offset, and not the actual time. I posed this issue to \#rubyonrails. Possibly the most accurate response was, "Something is f'd up."

So, would you like to see what happens when we tell Rails we want to default to Pacific Time (US & Canada), just for fun?

    # If config.time_zone is set to "Pacific Time (US & Canada)"
    >> t.created_at
    => Tue, 05 Aug 2008 10:38:55 PDT -07:00 # NOT correct -- it's 6:38 PDT -07:00
    >> t.created_at_before_type_cast
    => Tue, 05 Aug 2008 13:38:55 +0000 # Correct -- UTC
    >> Time.now
    => Tue Aug 05 09:40:02 -0400 2008 # Correct for my local time
    >> Time.zone.now
    => Tue, 05 Aug 2008 06:39:57 PDT -07:00 # Correct

What\!? Rails thinks that EDT == UTC, and the difference between EDT and PDT is 3 hours, so just subtract 3 hours from whatever we get back from the database.  
F'd up indeed\!

I gave up. I may have given up too early. Perhaps I could have figured it out. Frankly, there were better things to do. We just store everything in local time. For now, it won't hurt anything. In the future, it could be quite a hindrance.

Do you have any idea what is wrong? I'll buy you a beer if you find a solution.
