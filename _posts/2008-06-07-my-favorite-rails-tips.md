--- 
layout: post
title: My Favorite Rails Tips
date: 2008-06-07 23:25:11 UTC
comments: true
categories: 
--- 
<div class="centerize">

![screwdriver drill bit tips](http://soyunperdedor.com/files/drill_bit_tips.jpg "Photo By Dion Gillard")  
[Photo By Dion Gillard](http://www.flickr.com/photos/diongillard/ "Photo By Dion Gillard")

</div>

The [Rails tips contest](http://railscasts.com/contest/ "Rails tips contest") held by Ryan Bates at [Railscasts.com](http://railscasts.com/ "railscasts.com") recently produced a great deal of good tips to use when developing a Rails application. I love this sort of thing: Sharp little bits of Ruby or Rails goodness that are fairly easy to find from one page. It's a nice little collection of drill bits for me, the drill. (So, I'm a tool.) You, dear reader, should check out at least those who finished in the top ten, if not all of the submitted material. Nearly all of them are well worth a skim.

The following are some of my favorite tips from the contest:

## Easy SEO

The abbreviation SEO typically makes me wary. I perceive a lot of deceptive, dirty practices in association with 'SEO', but it doesn't have to be that way. There are some fully white-hat practices of which you can take advantage. There were a couple tips related to these in the contest.

Alastair Brunton recommends a simple way to include an article's headline in the URL:

> A very cheap way to generate more descriptive urls is to take advantage of the way that ruby parses numbers. It will strip everything which is not a number if the string starts with a number.

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

<span style="color: #9966CC; font-weight: bold;">class</span> Article \&lt; ActiveRecord::Base  
  <span style="color: #9966CC; font-weight: bold;">def</span> to\_param  
    <span style="color: #996600;">"\#{id}-\#{headline.gsub(/\[^a-z0-9\]+/i, '-')}"</span>.<span style="color: #9900CC;">downcase</span>  
  <span style="color: #9966CC; font-weight: bold;">end</span>  
<span style="color: #9966CC; font-weight: bold;">end</span>

</div>

</div>

This will produce URLs that look like 'http://soyunperdedor.com/articles/4-sensational-headline-a-la-michael-arrington'. The headline portion of the URL will get stripped and you will indeed still be able to use params\[:id\] \#=\> 4. Sweet.

Visit Mr. Brunton's [5 Rails Tips](http://scoop.cheerfactory.co.uk/2008/04/25/5-rails-tips/ "Brunton's 5 Rails Tips") for more information, and more tips.

Doug (maybe [this Doug?](http://i3.iofferphoto.com/img/item/183/309/86/Doug_cartoon.gif "Doug")) has another recommendation related to URLs and SEO. Assuming you're running Rails 2.1, you can now alias your resources in your routes file. For instance, as Doug suggested:

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

map.<span style="color: #9900CC;">resources</span> :operating\_systems, :as =\> 'operating-systems'

</div>

</div>

The purpose? Hyphens are more search engine friendly than underscores. See Doug's [5 Rails Tips](http://domainspecific.blogspot.com/2008/05/5-rails-tips.html "Doug's 5 Rails Tips") for more information.

## Annotating Models

Doug also recommends Dave Thomas' 'annotate\_models' plugin. I'm not sure how I missed this, but I like it. Like Doug, one of my favorite DataMapper features is that the model's schema is displayed within the model definition. Thanks to that, I don't have to look back at a migration, or at the table itself in some DB tool, when I forget the schema (which happens all of the time).

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

ruby script/plugin install http://repo.<span style="color: #9900CC;">pragprog</span>.<span style="color: #9900CC;">com</span>/svn/Public/plugins/annotate\_models

</div>

</div>

## Try, try() Again

Andreas Neuhaus suggests using [Chris Wanstrath's try()](http://ozmm.org/posts/try.html "Chris Wanstrath's try() method") to avoid sending messages to nil. This method is used in the GitHub code, as Chris says. A while back, there were several suggestions on what the best way to do this would be. For instance, [Reginald Braithwaite's andand()](http://andand.rubyforge.org/ "Reginald Braithwaite's andand() method") is another popular solution, and installing the andand gem gets you Ruby 1.9's Object\#tap (only better\!), as well.

For me, try() goes down smoother. I recommend checking out [Andreas' Five Tips for Developing Rails Applications](http://zargony.com/2008/04/28/five-tips-for-developing-rails-applications "Andreas' Five Tips for Developing Rails Applications") for some other great tips, in addition to the try() tip.

## TODO, FIXME, OPTIMIZE

Andreas also mentions that Rails includes a few rake tasks that make finding things needing done in your code pretty easy, as long as you left yourself some clues.

  - rake notes:todo - list all TODO comments
  - rake notes:fixme - list all FIXME comments
  - rake notes:optimize - list all OPTIMIZE comments
  - rake notes - list all of the above comments

I find these very handy, especially when I'm implementing or fixing something, and I notice that something else could be done different or better. I just make a note and continue in my work, so I don't lose my concentration. In fact, I made these into sake tasks so that they could be used elsewhere with ease. You're welcome to install them:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sake -i http://pastie.org/<span style="color: #cc66cc;">210890</span>.txt

</div>

</div>

## Gray's Chunky Finds

James Edward Gray II suggests a library he wrote and uses in many of his Rails apps which allows you to get a large data set in chunks, potentially increasing performance. I found it quite interesting. I haven't tried it, or benchmarked it yet, but I think it may find its way into our app at work soon. The function looks like this:

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

<span style="color: #9966CC; font-weight: bold;">module</span> FindAllInChunks  
  <span style="color: #9966CC; font-weight: bold;">def</span> find\<em\>all\</em\>in\<em\>chunks<span style="color: #006600; font-weight: bold;">(</span>query = Hash.<span style="color: #9900CC;">new</span>, \&amp;iterator<span style="color: #006600; font-weight: bold;">)</span>  
    <span style="color: #006666;">1</span>.<span style="color: #9900CC;">upto</span><span style="color: #006600; font-weight: bold;">(</span><span style="color: #006666;">1.0</span>/<span style="color: #006666;">0.0</span><span style="color: #006600; font-weight: bold;">)</span> <span style="color: #9966CC; font-weight: bold;">do</span> |i|  
      records = paginate<span style="color: #006600; font-weight: bold;">(</span><span style="color: #006600; font-weight: bold;">{</span>:page =\> i, :per\</em\>page =\> <span style="color: #006666;">50</span><span style="color: #006600; font-weight: bold;">}</span>.<span style="color: #9900CC;">merge</span><span style="color: #006600; font-weight: bold;">(</span>query<span style="color: #006600; font-weight: bold;">)</span><span style="color: #006600; font-weight: bold;">)</span>  
      records.<span style="color: #9900CC;">each</span><span style="color: #006600; font-weight: bold;">(</span>\&amp;iterator<span style="color: #006600; font-weight: bold;">)</span>  
      <span style="color: #9966CC; font-weight: bold;">break</span> <span style="color: #9966CC; font-weight: bold;">unless</span> records.<span style="color: #9900CC;">next\_page</span>  
    <span style="color: #9966CC; font-weight: bold;">end</span>  
  <span style="color: #9966CC; font-weight: bold;">end</span>  
end\</p\>  
  
\<p\>ActiveRecord::Base.<span style="color: #9900CC;">extend</span><span style="color: #006600; font-weight: bold;">(</span>FindAllInChunks<span style="color: #006600; font-weight: bold;">)</span>

</div>

</div>

It utilizes the will\_paginate plugin, by the way.

I recommend you visit \[Gray's tips\]\[Gray's Five ActiveRecord Tips\] for some other good tips, and to learn more about 'find*all*in\_chunks'. Let me know if you throw it an app immediately.

## The Command Line

Jerod Santo posted several tips in the Ruby on Rails forum. His first tip was to use GNU Screen, and I couldn't agree more. It's difficult to explain to people who haven't actually seen it in action. Basically, Screen allows me to split my terminal window so that the top portion is vim, the bottom portion is autotest, or possibly the 'tail -f' of the development log file, or the server output. Not only that, but I can have as many 'tabs' as I want, and everything can be accessed, moved, or modified through keyboard shortcuts. It's a wonderful thing.

To install on Ubuntu:  

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sudo apt-get install screen

</div>

</div>

You're welcome to use [my screenrc](http://github.com/mileszs/mileszs-linux-configs/tree/master/screenrc "my screenrc") (which is really Aaron Schaefer's screenrc, but [his Mercurial repo interface](http://projects.elasticdog.com/hg//configs/ "Aaron's Mercurial configs repo") confused me).

Check out [Santo's 5 Tips for RoR Beginners](http://railsforum.com/viewtopic.php?pid=60456). In particular, he includes a screencast showing GNU Screen in action, which is important.

Mikel Lindsaar 7th tip was all about command line shortcuts. Really, you should just go read [Lindsaar's Shell Shortcuts You Should Know and Love](http://lindsaar.net/2008/4/16/tip-7-shell-shortcuts-you-should-know-and-love). I am genuinely embarrassed to admit that I didn't know 'Ctrl-L' would clear the terminal window, as well as the IRB console. How did I not know that?

## A Page of Their Own

I really think someone should go through all of the submitted tips, mining as many as possible, eliminating the duplicates, and make them their own section on someone's site. At this point, it would likely be most logical for them to continue to live as a subsection of Railscasts, if Ryan wished to keep them. I suppose that creating a place for these to all live separate from their original site would also require permission from each author, which would make the whole process an even bigger pain in the ass.

Wouldn't it be nice to be able to access all of these tips within one unified interface? I think so.
