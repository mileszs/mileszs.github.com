--- 
layout: post
title: Uninitialized Constant MERB_ROOT Error
date: 2008-01-20 23:48:25 UTC
comments: true
categories: 
--- 
I intend to write articles covering *actual* programming soon, as opposed to the hodge-podge, link-filled, advocacy-style articles I have written up to now. In spite of that, I hope that the couple short blurbs about Rails or Merb errors do help a person or two. These little things can be frustrating as hell.

## Where Did This Error Come From?

If you have gotten this error, you are probably using Haml and Sass in your Merb application, right? You installed Haml, wrote a few views with it, and then decided to move on and generate the next controller or model, right? Right. This is probably what it looks like:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

>script/generate controller sessions  
  
Connecting to database...  
  
/usr/lib/ruby/gems/<span style="color: #cc66cc;">1.8</span>/gems/activesupport<span style="color: #cc66cc;">-2.0</span><span style="color: #cc66cc;">.2</span>/lib/  
active\_support/dependencies.rb  
:<span style="color: #cc66cc;">266</span>:<span style="color: #b1b100;">in</span> \`load\_missing\_constant<span style="color: #ff0000;">': uninitialized constant MERB\_ROOT (NameError)  
        from /usr/lib/ruby/gems/1.8/gems/activesupport-2.0.2/lib/  
active\_support/dependencies.rb  
:453:in \`const\_missing'</span>  
        from /usr/lib/ruby/gems/<span style="color: #cc66cc;">1.8</span>/gems/activesupport<span style="color: #cc66cc;">-2.0</span><span style="color: #cc66cc;">.2</span>/lib/  
active\_support/dependencies.rb  
:<span style="color: #cc66cc;">465</span>:<span style="color: #b1b100;">in</span> \`const\_missing\_before\_generators<span style="color: #ff0000;">'  
        from /usr/lib/ruby/gems/1.8/gems/rubigen-1.1.1/lib/rubigen/lookup.rb:13:in \`const\_missing'</span>  
        from /usr/lib/ruby/gems/<span style="color: #cc66cc;">1.8</span>/gems/haml<span style="color: #cc66cc;">-1.8</span><span style="color: #cc66cc;">.0</span>/lib/sass/plugin/merb.rb:<span style="color: #cc66cc;">13</span>  
        from /usr/<span style="color: #000066;">local</span>/lib/site\_ruby/<span style="color: #cc66cc;">1.8</span>/rubygems/custom\_require.rb:<span style="color: #cc66cc;">27</span>:<span style="color: #b1b100;">in</span> \`gem\_original\_require<span style="color: #ff0000;">'  
        from /usr/local/lib/site\_ruby/1.8/rubygems/custom\_require.rb:27:in \`require'</span>  
        from /usr/lib/ruby/gems/<span style="color: #cc66cc;">1.8</span>/gems/activesupport<span style="color: #cc66cc;">-2.0</span><span style="color: #cc66cc;">.2</span>/lib/  
active\_support/dependencies.rb  
:<span style="color: #cc66cc;">496</span>:<span style="color: #b1b100;">in</span> \`require<span style="color: #ff0000;">'  
        from /usr/lib/ruby/gems/1.8/gems/activesupport-2.0.2/lib/  
active\_support/dependencies.rb  
:342:in \`new\_constants\_in'</span>  
         ... <span style="color: #cc66cc;">23</span> levels...  
        from /usr/lib/ruby/gems/<span style="color: #cc66cc;">1.8</span>/gems/activesupport<span style="color: #cc66cc;">-2.0</span><span style="color: #cc66cc;">.2</span>/lib/  
active\_support/dependencies.rb  
:<span style="color: #cc66cc;">496</span>:<span style="color: #b1b100;">in</span> \`require<span style="color: #ff0000;">'  
        from /usr/lib/ruby/gems/1.8/gems/activesupport-2.0.2/lib/  
active\_support/dependencies.rb  
:342:in \`new\_constants\_in'</span>  
        from /usr/lib/ruby/gems/<span style="color: #cc66cc;">1.8</span>/gems/activesupport<span style="color: #cc66cc;">-2.0</span><span style="color: #cc66cc;">.2</span>/lib/  
active\_support/dependencies.rb  
:<span style="color: #cc66cc;">496</span>:<span style="color: #b1b100;">in</span> \`require<span style="color: #ff0000;">'  
        from script/generate:12</span>

</div>

</div>

## Crap. What do I do?

[Ardekantur knows what to do](http://blog.ardekantur.com/archives/27). His blurb on the subject is what helped me. I ended up opening the Sass' Merb plugin file in Vim, and replacing it all with the trunk version.

Open the file.

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sudo vim /usr/lib/ruby/gems/<span style="color: #cc66cc;">1.8</span>/gems/haml<span style="color: #cc66cc;">-1.8</span><span style="color: #cc66cc;">.0</span>/lib/sass/plugin/merb.rb

</div>

</div>

It should look like this:

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

  <span style="color: #9966CC; font-weight: bold;">unless</span> <span style="color: #9966CC; font-weight: bold;">defined</span>?<span style="color: #006600; font-weight: bold;">(</span>Sass::MERB\_LOADED<span style="color: #006600; font-weight: bold;">)</span>  
    Sass::MERB\_LOADED = <span style="color: #0000FF; font-weight: bold;">true</span>  
  
    Sass::Plugin.<span style="color: #9900CC;">options</span>.<span style="color: #9900CC;">merge</span>\!<span style="color: #006600; font-weight: bold;">(</span>:template\_location  =\> MERB\_ROOT + '/public/stylesheet  
  s/sass',  
                                :css\_location       =\> MERB\_ROOT + '/public/stylesheet  
  s',  
                                :always\_check       =\> MERB\_ENV \!= <span style="color: #996600;">"production"</span>,  
                                :full\_exception     =\> MERB\_ENV \!= <span style="color: #996600;">"production"</span><span style="color: #006600; font-weight: bold;">)</span>  
    config = Merb::Plugins.<span style="color: #9900CC;">config</span><span style="color: #006600; font-weight: bold;">\[</span>:sass<span style="color: #006600; font-weight: bold;">\]</span> || Merb::Plugins.<span style="color: #9900CC;">config</span><span style="color: #006600; font-weight: bold;">\[</span><span style="color: #996600;">"sass"</span><span style="color: #006600; font-weight: bold;">\]</span> || <span style="color: #006600; font-weight: bold;">{</span><span style="color: #006600; font-weight: bold;">}</span>  
    config.<span style="color: #9900CC;">symbolize\_keys</span>\!  
    Sass::Plugin.<span style="color: #9900CC;">options</span>.<span style="color: #9900CC;">merge</span>\!<span style="color: #006600; font-weight: bold;">(</span>config<span style="color: #006600; font-weight: bold;">)</span>  
   
    <span style="color: #9966CC; font-weight: bold;">class</span> MerbHandler <span style="color: #008000; font-style: italic;">\# :nodoc:</span>  
      <span style="color: #9966CC; font-weight: bold;">def</span> process\_with\_sass<span style="color: #006600; font-weight: bold;">(</span>request, response<span style="color: #006600; font-weight: bold;">)</span>  
        Sass::Plugin.<span style="color: #9900CC;">update\_stylesheets</span> <span style="color: #9966CC; font-weight: bold;">if</span> Sass::Plugin.<span style="color: #9900CC;">options</span><span style="color: #006600; font-weight: bold;">\[</span>:always\_update<span style="color: #006600; font-weight: bold;">\]</span> || Sas  
  s::Plugin.<span style="color: #9900CC;">options</span><span style="color: #006600; font-weight: bold;">\[</span>:always\_check<span style="color: #006600; font-weight: bold;">\]</span>  
        process\_without\_sass<span style="color: #006600; font-weight: bold;">(</span>request, response<span style="color: #006600; font-weight: bold;">)</span>  
      <span style="color: #9966CC; font-weight: bold;">end</span>  
      alias\_method :process\_without\_sass, :process  
      alias\_method :process, :process\_with\_sass  
    <span style="color: #9966CC; font-weight: bold;">end</span>  
  <span style="color: #9966CC; font-weight: bold;">end</span>

</div>

</div>

Uh, go ahead and delete all that. Yeah, delete it all. Paste this in there:

<div class="codeblock">

<div class="ruby" style="font-family: monospace;">

<span style="color: #9966CC; font-weight: bold;">unless</span> <span style="color: #9966CC; font-weight: bold;">defined</span>?<span style="color: #006600; font-weight: bold;">(</span>Sass::MERB\_LOADED<span style="color: #006600; font-weight: bold;">)</span>  
  Sass::MERB\_LOADED = <span style="color: #0000FF; font-weight: bold;">true</span>  
  
  version = Merb::VERSION.<span style="color: #CC0066; font-weight: bold;">split</span><span style="color: #006600; font-weight: bold;">(</span>'.'<span style="color: #006600; font-weight: bold;">)</span>.<span style="color: #9900CC;">map</span> <span style="color: #006600; font-weight: bold;">{</span> |n| n.<span style="color: #9900CC;">to\_i</span> <span style="color: #006600; font-weight: bold;">}</span>  
  <span style="color: #9966CC; font-weight: bold;">if</span> version<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">0</span><span style="color: #006600; font-weight: bold;">\]</span> \<= <span style="color: #006666;">0</span> && version<span style="color: #006600; font-weight: bold;">\[</span><span style="color: #006666;">1</span><span style="color: #006600; font-weight: bold;">\]</span> \< <span style="color: #006666;">5</span>  
    root = MERB\_ROOT  
    env  = MERB\_ENV  
  <span style="color: #9966CC; font-weight: bold;">else</span>  
    root = Merb.<span style="color: #9900CC;">root</span>  
    env  = Merb.<span style="color: #9900CC;">environment</span>  
  <span style="color: #9966CC; font-weight: bold;">end</span>  
  
  Sass::Plugin.<span style="color: #9900CC;">options</span>.<span style="color: #9900CC;">merge</span>\!<span style="color: #006600; font-weight: bold;">(</span>:template\_location  =\> root + '/public/stylesheets/sass',  
                              :css\_location       =\> root + '/public/stylesheets',  
                              :always\_check       =\> env \!= <span style="color: #996600;">"production"</span>,  
                              :full\_exception     =\> env \!= <span style="color: #996600;">"production"</span><span style="color: #006600; font-weight: bold;">)</span>  
  config = Merb::Plugins.<span style="color: #9900CC;">config</span><span style="color: #006600; font-weight: bold;">\[</span>:sass<span style="color: #006600; font-weight: bold;">\]</span> || Merb::Plugins.<span style="color: #9900CC;">config</span><span style="color: #006600; font-weight: bold;">\[</span><span style="color: #996600;">"sass"</span><span style="color: #006600; font-weight: bold;">\]</span> || <span style="color: #006600; font-weight: bold;">{</span><span style="color: #006600; font-weight: bold;">}</span>  
  config.<span style="color: #9900CC;">symbolize\_keys</span>\!  
  Sass::Plugin.<span style="color: #9900CC;">options</span>.<span style="color: #9900CC;">merge</span>\!<span style="color: #006600; font-weight: bold;">(</span>config<span style="color: #006600; font-weight: bold;">)</span>  
  
  <span style="color: #9966CC; font-weight: bold;">class</span> MerbHandler <span style="color: #008000; font-style: italic;">\# :nodoc:</span>  
    <span style="color: #9966CC; font-weight: bold;">def</span> process\_with\_sass<span style="color: #006600; font-weight: bold;">(</span>request, response<span style="color: #006600; font-weight: bold;">)</span>  
      Sass::Plugin.<span style="color: #9900CC;">update\_stylesheets</span> <span style="color: #9966CC; font-weight: bold;">if</span> Sass::Plugin.<span style="color: #9900CC;">options</span><span style="color: #006600; font-weight: bold;">\[</span>:always\_update<span style="color: #006600; font-weight: bold;">\]</span> || Sass::Plugin.<span style="color: #9900CC;">options</span><span style="color: #006600; font-weight: bold;">\[</span>:always\_check<span style="color: #006600; font-weight: bold;">\]</span>  
      process\_without\_sass<span style="color: #006600; font-weight: bold;">(</span>request, response<span style="color: #006600; font-weight: bold;">)</span>  
    <span style="color: #9966CC; font-weight: bold;">end</span>  
    alias\_method :process\_without\_sass, :process  
    alias\_method :process, :process\_with\_sass  
  <span style="color: #9966CC; font-weight: bold;">end</span>  
<span style="color: #9966CC; font-weight: bold;">end</span>

</div>

</div>

Save that. Then try your script/generate line again. It should work now.

## Wait, Don't Go Yet\!

Really, [Ardekantur](http://blog.ardekantur.com/) did all the work. I sort of feel like I plagiarized this whole article. You should go ahead and check out his blog. In particular, he has written an article [Writing A Web Application In Merb, Part I](http://blog.ardekantur.com/archives/26), in which he exudes a test-first attitude, and uses RSpec, both of which are a **Good Thing**. I think I'm going to subscribe in anticipation of the next part (I assume II, or maybe J). You should at least take a look\!
