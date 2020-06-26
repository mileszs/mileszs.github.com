--- 
layout: post
title: Disabling Timestamps by Model in Rails 1.2
date: 2008-02-07 02:01:07 UTC
comments: true
categories: 
--- 
I recently had a need to disable the timestamp for 'updated\_at' for one particular little update to a table. At work, we are stuck on Rails 1.2.x for the time being. ActiveRecord::Base does indeed have a class accessor called 'record\_timestamps' that can be set to true or false. However, in Rails 1.2.x, this is not class inheritable. So, I set about fixing that.

**Note:** If you are using Rails 2.0.x, you can safely ignore this. You may want to read through [Evan Weaver's 'hacking activerecord's automatic timestamps'](http://blog.evanweaver.com/articles/2006/12/26/hacking-activerecords-automatic-timestamps/), though.

### Crap. Can't you just do this for me?

This is easy, I swear. Like I said, it's really already been done, so you don't have to do any revolutionary hacking of any sort. In fact, you just need to change a few characters on one line of one file in ActiveRecord's Timestamp class, just like they did in the [actual patch to Rails](http://dev.rubyonrails.org/changeset/8217)a.

### Where is this timestamp crap?

In your Rails app's root directory, open up 'vendor/rails/activerecord/lib/active\_record/timestamp.rb' file. (You did already [freeze your Rails application](http://softiesonrails.com/2008/1/3/freezing-your-rails-application), right?) Find the following line:

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

base.<span style="color: #9900CC;">cattr\_accessor</span> :record\_timestamps, :instance\_writer =\> <span style="color: #0000FF; font-weight: bold;">false</span>

</div>

</div>

The part we are interested in changing is 'cattr\_accessor'. We want to make it class inheritable, and 'cattr\_accessor' does not allow such behavior, as [Dr. Nic pointed out, once upon a time](http://drnicwilliams.com/2006/08/27/so-cattr_accessor-doesnt-work-like-it-should/). Luckily, ActiveSupport provides 'class\_inheritable\_accessor'. So, change the above line to the following:

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

base.<span style="color: #9900CC;">class\_inheritable\_accessor</span> :record\_timestamps, :instance\_writer =\> <span style="color: #0000FF; font-weight: bold;">false</span>

</div>

</div>

### Uh, what now?

Good. Now, I created a class method in a model that should be used as a block to turn off timestamps for any table updates within.

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

<span style="color: #9966CC; font-weight: bold;">class</span> SuperBowlLosers \< ActiveRecord::Base  
  
  <span style="color: #9966CC; font-weight: bold;">def</span> skip\_stampings  
    <span style="color: #0000FF; font-weight: bold;">self</span>.<span style="color: #9900CC;">record\_timestamps</span> = <span style="color: #0000FF; font-weight: bold;">false</span>  
    <span style="color: #9966CC; font-weight: bold;">yield</span>  
    <span style="color: #0000FF; font-weight: bold;">self</span>.<span style="color: #9900CC;">record\_timestamps</span> = <span style="color: #0000FF; font-weight: bold;">true</span>  
  <span style="color: #9966CC; font-weight: bold;">end</span>  
  
<span style="color: #9966CC; font-weight: bold;">end</span>

</div>

</div>

Now, elsewhere, I can make a minor update to a field without updating the 'updated\_at' portion.

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

  <span style="color: #008000; font-style: italic;">\# Should probably be done every few minutes</span>  
  @pats = SuperBowlLosers.<span style="color: #9900CC;">find\_by\_year</span><span style="color: #006600; font-weight: bold;">(</span><span style="color: #006666;">2008</span><span style="color: #006600; font-weight: bold;">)</span>  
  SuperBowlLosers.<span style="color: #9900CC;">skip\_stampings</span> <span style="color: #9966CC; font-weight: bold;">do</span>  
    @pats.<span style="color: #9900CC;">update\_attribute</span><span style="color: #006600; font-weight: bold;">(</span>:pissing\_and\_moaning\_and\_crying\_at, Time.<span style="color: #9900CC;">now</span><span style="color: #006600; font-weight: bold;">)</span>  
  <span style="color: #9966CC; font-weight: bold;">end</span>

</div>

</div>

### TA DA\!

See? That wasn't so bad, was it? For methods other than my 'skip\_stampings' method (that I did not dream up myself, as you'll see), check out [Evan Weaver's 'hacking activerecord's automatic timestamps'](http://blog.evanweaver.com/articles/2006/12/26/hacking-activerecords-automatic-timestamps/).
