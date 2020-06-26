--- 
layout: post
title: Thor, Sake, and Rails Log Stats
date: 2008-06-10 21:41:24 UTC
comments: true
categories: 
--- 
<div class="centerize">

![Thor Action Figure](http://soyunperdedor.com/files/thor.jpg "Thor and Mjolnir!")  
[Photo By Ben Northern](http://www.flickr.com/photos/bnorthern/)

</div>

Of course, converting the Rake task from [my previous post](http://soyunperdedor.com/node/27) to a Sake task was simple.

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

namespace :log <span style="color: #9966CC; font-weight: bold;">do</span>  
  
  desc "Show average reqs/sec of provided log file. <span style="color: #9900CC;">Usage</span>: sake log:stats FILE=/path/to/file"  
  task :stats <span style="color: #9966CC; font-weight: bold;">do</span> |task, args|  
    file = ENV<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #996600;">"FILE"</span><span style="color: #006600; font-weight: bold;">\]</span>  
    <span style="color: #CC0066; font-weight: bold;">puts</span> <span style="color: #996600;">"Parsing \#{file}..."</span>  
  
    <span style="color: #008000; font-style: italic;">\# Log Analyzing Task </span>  
    log = File.<span style="color: #CC0066; font-weight: bold;">open</span><span style="color: #006600; font-weight: bold;">(</span>file<span style="color: #006600; font-weight: bold;">)</span>  
  
    reqs\_per\_sec = <span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006600; font-weight: bold;">\]</span>  
  
    log.<span style="color: #9900CC;">each\_line</span> <span style="color: #9966CC; font-weight: bold;">do</span> |line|  
      parts = line.<span style="color: #9900CC;">scan</span><span style="color: #006600; font-weight: bold;">(</span>/<span style="color: #006600; font-weight: bold;">(</span>\\d+<span style="color: #006600; font-weight: bold;">)</span><span style="color: #006600; font-weight: bold;">(</span>\\sreqs\\/sec<span style="color: #006600; font-weight: bold;">)</span>/<span style="color: #006600; font-weight: bold;">)</span>  
      <span style="color: #008000; font-style: italic;">\# There should only be one match per line... right? </span>  
      reqs\_per\_sec \<\< parts<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">0</span><span style="color: #006600; font-weight: bold;">\]</span><span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">0</span><span style="color: #006600; font-weight: bold;">\]</span>.<span style="color: #9900CC;">to\_i</span> <span style="color: #9966CC; font-weight: bold;">unless</span> parts<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">0</span><span style="color: #006600; font-weight: bold;">\]</span>.<span style="color: #0000FF; font-weight: bold;">nil</span>?  
    <span style="color: #9966CC; font-weight: bold;">end</span>  
  
    <span style="color: #008000; font-style: italic;">\# Find the mode</span>  
    nums = <span style="color: #006600; font-weight: bold;">{</span><span style="color: #006600; font-weight: bold;">}</span>  
    reqs\_per\_sec.<span style="color: #9900CC;">each</span> <span style="color: #9966CC; font-weight: bold;">do</span> |r|  
      <span style="color: #9966CC; font-weight: bold;">unless</span> nums.<span style="color: #9966CC; font-weight: bold;">include</span>?<span style="color: #006600; font-weight: bold;">(</span>r<span style="color: #006600; font-weight: bold;">)</span>  
        nums<span style="color: #006600; font-weight: bold;">\[</span>r<span style="color: #006600; font-weight: bold;">\]</span> ||= <span style="color: #006666;">0</span>  
        nums<span style="color: #006600; font-weight: bold;">\[</span>r<span style="color: #006600; font-weight: bold;">\]</span> += <span style="color: #006666;">1</span>  
      <span style="color: #9966CC; font-weight: bold;">end</span>  
    <span style="color: #9966CC; font-weight: bold;">end</span>  
    mode = nums.<span style="color: #9900CC;">sort</span> <span style="color: #006600; font-weight: bold;">{</span>|a, b| a<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">1</span><span style="color: #006600; font-weight: bold;">\]</span> \<=\> b<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">1</span><span style="color: #006600; font-weight: bold;">\]</span> <span style="color: #006600; font-weight: bold;">}</span>.<span style="color: #9900CC;">last</span><span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">0</span><span style="color: #006600; font-weight: bold;">\]</span>  
  
    sum = reqs\_per\_sec.<span style="color: #9900CC;">inject</span> <span style="color: #006600; font-weight: bold;">{</span>|sum, x| sum + x <span style="color: #006600; font-weight: bold;">}</span>  
  
    <span style="color: #CC0066; font-weight: bold;">puts</span> <span style="color: #CC0066; font-weight: bold;">sprintf</span><span style="color: #006600; font-weight: bold;">(</span><span style="color: #996600;">"%-7s %d reqs/sec"</span>, "Mean:", sum/reqs\_per\_sec.<span style="color: #9900CC;">size</span><span style="color: #006600; font-weight: bold;">)</span>  
    <span style="color: #CC0066; font-weight: bold;">puts</span> <span style="color: #CC0066; font-weight: bold;">sprintf</span><span style="color: #006600; font-weight: bold;">(</span><span style="color: #996600;">"%-7s %d reqs/sec"</span>, "Median:", reqs\_per\_sec<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006600; font-weight: bold;">(</span>reqs\_per\_sec.<span style="color: #9900CC;">size</span>/<span style="color: #006666;">2</span><span style="color: #006600; font-weight: bold;">)</span>.<span style="color: #9900CC;">to\_i</span><span style="color: #006600; font-weight: bold;">\]</span><span style="color: #006600; font-weight: bold;">)</span>  
    <span style="color: #CC0066; font-weight: bold;">puts</span> <span style="color: #CC0066; font-weight: bold;">sprintf</span><span style="color: #006600; font-weight: bold;">(</span><span style="color: #996600;">"%-7s %d reqs/sec"</span>, "Mode:", mode<span style="color: #006600; font-weight: bold;">)</span>  
  <span style="color: #9966CC; font-weight: bold;">end</span>  
<span style="color: #9966CC; font-weight: bold;">end</span>

</div>

</div>

So...

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

> sake -i http://pastie.org/<span style="color: #cc66cc;">212542</span>.txt  
> sake log:reqs\_stats <span style="color: #0000ff;">FILE=</span>/path/to/file  
Parsing production.log...  
Mean:   <span style="color: #cc66cc;">55</span> reqs/sec  
Median: <span style="color: #cc66cc;">90</span> reqs/sec  
Mode:   <span style="color: #cc66cc;">110</span> reqs/sec

</div>

</div>

## Stand and Face the Might of Thor\!

[Thor](http://yehudakatz.com/2008/05/12/by-thors-hammer/) purports to replace Rake and Sake, at least for system scripting. It can do the same things Rake and Sake can do, but it does them while actually looking like (mostly) plain Ruby, and, on top of that, it makes dealing with command line options and argument super freakin' simple. I'll admit, I was on the fence about Thor until actually writing this script:

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

<span style="color: #008000; font-style: italic;">\# module: rlog</span>  
  
<span style="color: #9966CC; font-weight: bold;">class</span> RLog \< Thor  
  
  desc 'stats FILE', 'view reqs/sec stats from a Rails log'  
  <span style="color: #9966CC; font-weight: bold;">def</span> stats<span style="color: #006600; font-weight: bold;">(</span>file<span style="color: #006600; font-weight: bold;">)</span>  
    <span style="color: #CC0066; font-weight: bold;">puts</span> <span style="color: #996600;">"Parsing \#{file}..."</span>  
  
    reqs\_per\_sec = parse\_log\_file<span style="color: #006600; font-weight: bold;">(</span>file<span style="color: #006600; font-weight: bold;">)</span>  
  
    <span style="color: #CC0066; font-weight: bold;">puts</span> <span style="color: #CC0066; font-weight: bold;">sprintf</span><span style="color: #006600; font-weight: bold;">(</span><span style="color: #996600;">"%-7s %d reqs/sec"</span>, 'Mean:', mean<span style="color: #006600; font-weight: bold;">(</span>reqs\_per\_sec<span style="color: #006600; font-weight: bold;">)</span><span style="color: #006600; font-weight: bold;">)</span>  
    <span style="color: #CC0066; font-weight: bold;">puts</span> <span style="color: #CC0066; font-weight: bold;">sprintf</span><span style="color: #006600; font-weight: bold;">(</span><span style="color: #996600;">"%-7s %d reqs/sec"</span>, 'Median:', median<span style="color: #006600; font-weight: bold;">(</span>reqs\_per\_sec<span style="color: #006600; font-weight: bold;">)</span><span style="color: #006600; font-weight: bold;">)</span>  
    <span style="color: #CC0066; font-weight: bold;">puts</span> <span style="color: #CC0066; font-weight: bold;">sprintf</span><span style="color: #006600; font-weight: bold;">(</span><span style="color: #996600;">"%-7s %d reqs/sec"</span>, 'Mode:', mode<span style="color: #006600; font-weight: bold;">(</span>reqs\_per\_sec<span style="color: #006600; font-weight: bold;">)</span><span style="color: #006600; font-weight: bold;">)</span>  
  <span style="color: #9966CC; font-weight: bold;">end</span>  
  
  private  
  
    <span style="color: #9966CC; font-weight: bold;">def</span> parse\_log\_file<span style="color: #006600; font-weight: bold;">(</span>file<span style="color: #006600; font-weight: bold;">)</span>  
      <span style="color: #008000; font-style: italic;">\# Log Analyzing Task </span>  
      log = File.<span style="color: #CC0066; font-weight: bold;">open</span><span style="color: #006600; font-weight: bold;">(</span>file<span style="color: #006600; font-weight: bold;">)</span>  
  
      reqs\_per\_sec = <span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006600; font-weight: bold;">\]</span>  
  
      log.<span style="color: #9900CC;">each\_line</span> <span style="color: #9966CC; font-weight: bold;">do</span> |line|  
        parts = line.<span style="color: #9900CC;">scan</span><span style="color: #006600; font-weight: bold;">(</span>/<span style="color: #006600; font-weight: bold;">(</span>\\d+<span style="color: #006600; font-weight: bold;">)</span><span style="color: #006600; font-weight: bold;">(</span>\\sreqs\\/sec<span style="color: #006600; font-weight: bold;">)</span>/<span style="color: #006600; font-weight: bold;">)</span>  
        <span style="color: #008000; font-style: italic;">\# There should only be one match per line... right? </span>  
        reqs\_per\_sec \<\< parts<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">0</span><span style="color: #006600; font-weight: bold;">\]</span><span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">0</span><span style="color: #006600; font-weight: bold;">\]</span>.<span style="color: #9900CC;">to\_i</span> <span style="color: #9966CC; font-weight: bold;">unless</span> parts<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">0</span><span style="color: #006600; font-weight: bold;">\]</span>.<span style="color: #0000FF; font-weight: bold;">nil</span>?  
      <span style="color: #9966CC; font-weight: bold;">end</span>  
  
      reqs\_per\_sec  
    <span style="color: #9966CC; font-weight: bold;">end</span>  
  
    <span style="color: #9966CC; font-weight: bold;">def</span> mean<span style="color: #006600; font-weight: bold;">(</span>reqs\_per\_sec<span style="color: #006600; font-weight: bold;">)</span>  
      sum = reqs\_per\_sec.<span style="color: #9900CC;">inject</span> <span style="color: #006600; font-weight: bold;">{</span>|sum, x| sum + x <span style="color: #006600; font-weight: bold;">}</span>  
      sum/reqs\_per\_sec.<span style="color: #9900CC;">size</span>  
    <span style="color: #9966CC; font-weight: bold;">end</span>  
  
    <span style="color: #9966CC; font-weight: bold;">def</span> median<span style="color: #006600; font-weight: bold;">(</span>reqs\_per\_sec<span style="color: #006600; font-weight: bold;">)</span>  
      sum = reqs\_per\_sec.<span style="color: #9900CC;">inject</span> <span style="color: #006600; font-weight: bold;">{</span>|sum, x| sum + x <span style="color: #006600; font-weight: bold;">}</span>  
      reqs\_per\_sec<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006600; font-weight: bold;">(</span>reqs\_per\_sec.<span style="color: #9900CC;">size</span>/<span style="color: #006666;">2</span><span style="color: #006600; font-weight: bold;">)</span>.<span style="color: #9900CC;">to\_i</span><span style="color: #006600; font-weight: bold;">\]</span>  
    <span style="color: #9966CC; font-weight: bold;">end</span>  
  
    <span style="color: #9966CC; font-weight: bold;">def</span> mode<span style="color: #006600; font-weight: bold;">(</span>reqs\_per\_sec<span style="color: #006600; font-weight: bold;">)</span>  
      <span style="color: #008000; font-style: italic;">\# Find the mode</span>  
      nums = <span style="color: #006600; font-weight: bold;">{</span><span style="color: #006600; font-weight: bold;">}</span>  
      reqs\_per\_sec.<span style="color: #9900CC;">each</span> <span style="color: #9966CC; font-weight: bold;">do</span> |r|  
        <span style="color: #9966CC; font-weight: bold;">unless</span> nums.<span style="color: #9966CC; font-weight: bold;">include</span>?<span style="color: #006600; font-weight: bold;">(</span>r<span style="color: #006600; font-weight: bold;">)</span>  
          nums<span style="color: #006600; font-weight: bold;">\[</span>r<span style="color: #006600; font-weight: bold;">\]</span> ||= <span style="color: #006666;">0</span>  
          nums<span style="color: #006600; font-weight: bold;">\[</span>r<span style="color: #006600; font-weight: bold;">\]</span> += <span style="color: #006666;">1</span>  
        <span style="color: #9966CC; font-weight: bold;">end</span>  
      <span style="color: #9966CC; font-weight: bold;">end</span>  
      nums.<span style="color: #9900CC;">sort</span> <span style="color: #006600; font-weight: bold;">{</span>|a, b| a<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">1</span><span style="color: #006600; font-weight: bold;">\]</span> \<=\> b<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">1</span><span style="color: #006600; font-weight: bold;">\]</span> <span style="color: #006600; font-weight: bold;">}</span>.<span style="color: #9900CC;">last</span><span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">0</span><span style="color: #006600; font-weight: bold;">\]</span>  
    <span style="color: #9966CC; font-weight: bold;">end</span>  
  
<span style="color: #9966CC; font-weight: bold;">end</span>

</div>

</div>

I am going to assume you haven't installed Thor. If you have, you know which step to skip:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

> sudo gem install thor  
> thor install http://pastie.org/<span style="color: #cc66cc;">212189</span>.txt  
> thor list  
Tasks  
\-----  
r\_log:stats FILE   view reqs/sec stats from a Rails log  
> thor r\_log:stats /path/to/file  
Parsing production.log...  
Mean:   <span style="color: #cc66cc;">55</span> reqs/sec  
Median: <span style="color: #cc66cc;">90</span> reqs/sec  
Mode:   <span style="color: #cc66cc;">110</span> reqs/sec

</div>

</div>

## And verily, thine hour of judgment is at hand\!

Thor is cool. There's no doubt about it. It does indeed make dealing with options in Ruby scripts trivial (although I obviously haven't really used that yet), and it's nice that it is less DSL and more plain Ruby. I don't understand why Thor hasn't gotten the same amount of love that Sake got when it was debuted. It makes me wonder if people love the Rake DSL more than they love Ruby. **Sinners\!** **Blasphemers\!** **Hipsters\!** If I can make Thor do anything I would want to do with Sake, or, even better, anything I would want to do with any old bash script, then I am sure I will be using Thor quite often. **You** should, too.

Go checkout wycats [initial post about Thor](http://yehudakatz.com/2008/05/12/by-thors-hammer/). Also, go look at [the Thor source on GitHub](http://github.com/wycats/thor/tree/master). Write a couple Thor scripts and tell me what you think. Or, tell wycats what you think.

## Art Thou Mad?

Where was I? Oh, right, the Rails log stat scripts. Yup. There they are. What do you think? Seriously, what could be added (that it would make sense to add to a Rake/Sake/Thor script)? What have I done horribly wrong? How poor is my Rubyisms knowledge? **Tell me**\!

I'll leave you with the most common Thor (from Marvel Comics, if you hadn't gotten it by now) phrase:

## I Say Thee Nay\!
