--- 
layout: post
title: Rake Rails Log Stats
date: 2008-06-10 00:07:11 UTC
comments: true
categories: 
--- 
<div class="centerize">

![Stack of Logs](http://soyunperdedor.com/files/logs.jpg "Logs!  Everywhere!")  
[Photo By mer de glace](http://flickr.com/photos/marts-pics)

</div>

There are several log analyzers for all sorts of logs. You can find analyzers for Apache logs, Nginx logs, auth logs, probably fail2ban logs, and on and on. Of course, you can also find several analyzers for Rails logs: [Rawk](http://ckhsponge.wordpress.com/2006/10/11/ruby-on-rails-log-analyzer-rawk/), [Rails Analyzer Tools](http://rubyforge.org/projects/rails-analyzer) (optionally with the [Hodel3000CompliantLogger](http://nubyonrails.com/articles/a-hodel-3000-compliant-logger-for-the-rest-of-us)), [Watson](http://watson.rubyforge.org/), and [LogJuicer](http://logjuicer.org/).

Today, all I wanted to know was the average requests per second for our production app. "I'm sure someone has written a rake or sake task to do this already," I thought, as I typed my search in to YubNub. I was wrong. As far as I can tell from a bit of googling, no one has freakin' done it. So, I did.

## Give Me the Goods

A simple rake task for getting the mean, median, and mode requests per second in your production log:

    namespace :log do
    
      desc 'Show average reqs/sec'
      task :reqs_stats do
        
        puts 'Parsing log/production.log...'
    
        # Log Analyzing Task
        log = File.open("#{RAILS_ROOT}/log/production.log")
         
        reqs_per_sec = []
       
        log.each_line do |line|
          parts = line.scan(/(\d+)(\sreqs\/sec)/)
          # There should only be one match per line... right?
          reqs_per_sec << parts[0][0].to_i unless parts[0].nil?
        end   
    
        # Find the mode
        nums = {}
        reqs_per_sec.each do |r|
          unless nums.include?(r)
            nums[r] ||= 0
            nums[r] += 1
          end   
        end   
        mode = nums.sort {|a, b| a[1] <=> b[1] }.last[0]
        
        sum = reqs_per_sec.inject {|sum, x| sum + x }
    
        puts sprintf("%-7s %d reqs/sec", 'Mean:', sum/reqs_per_sec.size)
        puts sprintf("%-7s %d reqs/sec", 'Median:', reqs_per_sec[(reqs_per_sec.size/2).to_i])
        puts sprintf("%-7s %d reqs/sec", 'Mode:', mode)
      end
    end

As you can tell, it's pretty simple. Read each line of the production log, parse out the number next to 'reqs/sec', throw them in an array. Calculate.

## Dude, You Can Do Better

It could be better. For instance, I could scope it by environment. That wouldn't be too difficult, as several rake tasks do this already. However, I would argue that parsing the development log is rarely necessary, nor is it all that useful. I could probably move each calculation (mean, median, and mode) to separate tasks, so that you can choose to call just one. When would you want to call just one, though?

The first improvement I would make would be to mold it into a useable sake task. Mostly, this would mean simply allowing it to take a file as an argument, which is simple enough. I will probably do this sooner rather than later. I promise I'll post it.

What would you do to improve it? I'm not an expert of anything. I can always learn more, and would greatly appreciate some schooling. (Or, at least suggestions.) So bring it on.
