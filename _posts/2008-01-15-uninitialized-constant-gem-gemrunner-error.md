--- 
layout: post
title: Uninitialized Constant Gem::GemRunner Error
date: 2008-01-15 02:18:23 UTC
comments: true
categories: 
--- 
Recently, I was attempting to freeze a Rails app at work. When I attempted to run `rake rails:freeze:gems`, I received this fun error:

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

uninitialized constant Gem::GemRunner

</div>

</div>

I had heard some rumor of an issue with Gem version 1.0.x and Rails versions less than 2.0. I was hoping that some magical silicon gnome would smooth out the wrinkles before I got a chance to trip over one, but alas, that was not the case.

Apparently, since Gem version 0.9.5, gem\_runner is not required automatically for the rake tasks. The simple solution is to edit your framework.rake file, and add the require line in the rails:freeze:gems task. (For me, located in '/usr/lib/ruby/gems/1.8/gems/rails-2.0.2/lib/tasks'.) It should end up looking something like this:

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

namespace :rails <span style="color: #9966CC; font-weight: bold;">do</span>  
  namespace :freeze <span style="color: #9966CC; font-weight: bold;">do</span>  
    desc <span style="color: #996600;">"Lock this application to the current gems (by unpacking them into vendor/rails)"</span>  
    task :gems <span style="color: #9966CC; font-weight: bold;">do</span>  
      deps = %w<span style="color: #006600; font-weight: bold;">(</span>actionpack activerecord actionmailer activesupport activeresource<span style="color: #006600; font-weight: bold;">)</span>  
      <span style="color: #CC0066; font-weight: bold;">require</span> 'rubygems'  
      <span style="color: #CC0066; font-weight: bold;">require</span> 'rubygems/gem\_runner'  
      Gem.<span style="color: #9900CC;">manage\_gems</span>

</div>

</div>

I hope that helps someone.
