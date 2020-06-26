--- 
layout: post
title: From class_eval to Modules
date: 2008-09-14 14:52:14 UTC
comments: true
categories: 
--- 
Once in a while, it's nice to actually go back and look at old code. Not because that old code makes you feel good about yourself at all -- quite the opposite -- but because you feel good about yourself after you clean it up a little bit.

Ola Bini's [Evil Hook Methods?](http://olabini.com/blog/2008/09/evil-hook-methods/) got me thinking about how I extend classes in a dusty Rails application. It reminded me of Jay Fields' [Ruby: Underuse of Modules](http://blog.jayfields.com/2008/07/ruby-underuse-of-modules.html). Of course, Jay also [weighed in on](http://blog.jayfields.com/2008/09/domain-specific-languages-dont-follow.html) Ola's concerns. All of these articles have led me to take a look at some simple extensions I had made to Time for this particular Dusty Rails App (DRA, from now on). First, this is what I had before:

    Time.class_eval do 
      def self.next_sunday
        t = Time.local(Time.now.year, Time.now.month, Time.now.day, 0) # Today at midnight
        t += 60*60*24 until t.wday == 0
        t # Should now be next Sunday at midnight
      end
    
      def self.nth_wday(n, wday, month, year)
        if (!n.between? 1,5) ||
           (!wday.between? 0,6) ||
           (!month.between? 1,12)
          raise ArgumentError
        end
        t = Time.local year, month, 1
        first = t.wday
        if first == wday
          fwd = 1
        elsif first < wday
          fwd = wday - first + 1
        elsif first > wday
          fwd = (wday+7) - first + 1
        end
        target = fwd + (n-1)*7
        begin
          t2 = Time.local year, month, target
        rescue ArgumentError
          return nil
        end
        if t2.mday == target
          t2
        else
          nil
        end
      end
    
      def after_hours?
        result = true
        # M-F, 8am to 6pm are business hours
        if (1..5).include?(self.wday) && (8..18).include?(self.hour) 
          result = false
        end
        # unless it's a holiday!
        if self.holiday?
          result = true
        end
        result
      end
    
      def holiday?
        unless new_years_day? || 
           mlk_jr_day? ||
           memorial_day? ||
           independence_day? ||
           labor_day? ||
           thanksgiving? ||
           christmas_day?
          return false
        end
        true
      end
    
      def new_years_day?
        return false unless self.yday == 1
        true
      end
    
      def mlk_jr_day?
        return false unless self.yday == Time.nth_wday(3,1,1,self.year)
        true
      end
    
      def memorial_day?
        last_day_of_month = Time.local(self.year, 5, 31)
        d = last_day_of_month.mday - last_day_of_month.wday + 1
        return false unless Time.now.yday == Time.local(self.year, 5, d)
        true
      end
    
      def indepence_day?
        return false unless self.month == 7 && self.mday == 4
        true
      end
    
      def labor_day?
        return false unless self.yday == Time.nth_wday(1,1,9,self.year).yday
        true
      end
    
      def thanksgiving?
        return false unless self.yday == self.class.nth_wday(4,4,11,self.year).yday 
        true
      end
    
      def christmas_day?
        return false unless self.month == 12 && self.mday == 25
        true
      end
    end

## I see only trees. Where is the forest?

What we're worried about is **how** Time was extended. It was a simple class\_eval -- just open the class up and insert these methods. The file is simply required in config/environment.rb. In this case, I am adding functionality that didn't exist, so it could be left alone. It's not that big of a deal. As long as you're looking at the file that contains this code, understanding what is happening is pretty simple.

I would like to move these methods to a module. Time could then include/extend them. I do like to have modules in the lib/ directory while using initializers to pull them into DRA. That method of organization seems much cleaner to me. Plus, I can't very well end this article by telling you that I'm just going to leave DRA alone. Not now that we've come this far.

## Modularize It Already\!

    module NframeTimeExtensions
      module ClassMethods
        def next_sunday
          t = Time.local(Time.now.year, Time.now.month, Time.now.day, 0) # Today at midnight
          t += 60*60*24 until t.wday == 0
          t # Should now be next Sunday at midnight
        end
    
        def nth_wday(n, wday, month, year)
          if (!n.between? 1,5) ||
             (!wday.between? 0,6) ||
             (!month.between? 1,12)
            raise ArgumentError
          end
          t = Time.local year, month, 1
          first = t.wday
          if first == wday
            fwd = 1
          elsif first < wday
            fwd = wday - first + 1
          elsif first > wday
            fwd = (wday+7) - first + 1
          end
          target = fwd + (n-1)*7
          begin
            t2 = Time.local year, month, target
          rescue ArgumentError
            return nil
          end
          if t2.mday == target
            t2
          else
            nil
          end
        end
      end
    
      def after_hours?
        result = true
        # M-F, 8am to 6pm are business hours
        if (1..5).include?(self.wday) && (8..18).include?(self.hour) 
          result = false
        end
        # unless it's a holiday!
        if self.holiday?
          result = true
        end
        result
      end
    
      def holiday?
        unless new_years_day? || 
           mlk_jr_day? ||
           memorial_day? ||
           independence_day? ||
           labor_day? ||
           thanksgiving? ||
           christmas_day?
          return false
        end
        true
      end
    
      def new_years_day?
        return false unless self.yday == 1
        true
      end
    
      def mlk_jr_day?
        return false unless self.yday == Time.nth_wday(3,1,1,self.year)
        true
      end
    
      def memorial_day?
        last_day_of_month = Time.local(self.year, 5, 31)
        d = last_day_of_month.mday - last_day_of_month.wday + 1
        return false unless Time.now.yday == Time.local(self.year, 5, d)
        true
      end
    
      def indepence_day?
        return false unless self.month == 7 && self.mday == 4
        true
      end
    
      def labor_day?
        return false unless self.yday == Time.nth_wday(1,1,9,self.year).yday
        true
      end
    
      def thanksgiving?
        return false unless self.yday == self.class.nth_wday(4,4,11,self.year).yday 
        true
      end
    
      def christmas_day?
        return false unless self.month == 12 && self.mday == 25
        true
      end
    end

Ah, there. The module ClassMethods pattern is used throughout Rails, so you've likely seen it before. Other than that, it's pretty straight-forward. I hope.

Now I need to get it into DRA. So, in RAILS*ROOT/config/initializers/hey*lets*include*those*stupid*time\_extensions.rb:

    require 'nframe_time_extensions'
    
    Time.class_eval do
      include NframeTimeExtensions
      extend NframeTimeExtensions::ClassMethods
    end

That smells better to me.

## Whoa. Way More Code.

Yeah, this does take more code, and an extra file. I don't mind writing a bit of extra code when I believe that it will make the code clearer. As I said before, this method of organization seems much clearer to me.

It was also a fun little exercise in extending via modules instead of directly adding to classes. That is not a bad thing to practice.
