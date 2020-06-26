--- 
layout: post
title: Tweeting From the Command Line
date: 2008-03-30 01:29:38 UTC
comments: true
categories: 
--- 
A recent tweet from [Tim Bray](http://tbray.org/ongoing) contained a link to [an article explaining how to tweet from the command line](http://blogs.linux.org.bd/?p=6101). That prompted me to write a quick script to make tweeting from the command line a bit easier. You can can download the script: [tweet](http://soyunperdedor.com/files/tweet). You can grab it from GitHub:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

git clone git://github.com/MilesZS/command-line-tweet.git

</div>

</div>

You can see the script below:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

<span style="color: #808080; font-style: italic;">\#\!/bin/bash  
</span>  
<span style="color: #808080; font-style: italic;">\# Author: Miles Z. Sterrett \<miles -dot- sterrett AT gmail -dot- com\>  
</span><span style="color: #808080; font-style: italic;">\# Created: <span style="color: #cc66cc;">03</span><span style="color: #cc66cc;">-29</span><span style="color: #cc66cc;">-08</span>  
</span><span style="color: #808080; font-style: italic;">\# Updated: <span style="color: #cc66cc;">03</span><span style="color: #cc66cc;">-29</span><span style="color: #cc66cc;">-08</span>  
</span>  
<span style="color: #808080; font-style: italic;">\# Sends an update to Twitter  
</span>  
<span style="color: #808080; font-style: italic;">\# Uncomment and enter your own Twitter info  
</span><span style="color: #808080; font-style: italic;">\# to avoid having to use the <span style="color: #000066;">command</span> line parameters  
</span><span style="color: #808080; font-style: italic;">\#<span style="color: #0000ff;">USER=</span>username  
</span><span style="color: #808080; font-style: italic;">\#<span style="color: #0000ff;">PASS=</span>password  
</span>  
<span style="color: #b1b100;">function</span> force\_response <span style="color: #66cc66;">{</span>  
  <span style="color: #0000ff;">message=</span>$<span style="color: #cc66cc;">1</span>  
  <span style="color: #000066;">local</span> <span style="color: #0000ff;">answer=</span>  
  
  <span style="color: #000066;">read</span> -e -p <span style="color: #ff0000;">"$message"</span> answer  
  
  <span style="color: #b1b100;">while</span> <span style="color: #66cc66;">\[</span> -z <span style="color: #0000ff;">$answer</span> <span style="color: #66cc66;">\]</span>; <span style="color: #b1b100;">do</span>  
    <span style="color: #000066;">echo</span> <span style="color: #ff0000;">"You must enter a value to continue..."</span>  
    <span style="color: #000066;">read</span> -e -p <span style="color: #ff0000;">"$message"</span> answer  
  <span style="color: #b1b100;">done</span>  
  
  <span style="color: #000066;">eval</span> $<span style="color: #cc66cc;">2</span>=<span style="color: #0000ff;">$answer</span>  
<span style="color: #66cc66;">}</span>  
  
<span style="color: #b1b100;">while</span> <span style="color: #000066;">getopts</span> <span style="color: #ff0000;">"u:p:hm:"</span> OPTION  
<span style="color: #b1b100;">do</span>  
  <span style="color: #b1b100;">case</span> <span style="color: #0000ff;">$OPTION</span> <span style="color: #b1b100;">in</span>  
    u<span style="color: #66cc66;">)</span>   
      <span style="color: #0000ff;">USER=</span><span style="color: #0000ff;">$OPTARG</span>  
      ;;     
    p<span style="color: #66cc66;">)</span>   
      <span style="color: #0000ff;">PASS=</span><span style="color: #0000ff;">$OPTARG</span>  
      ;;     
    h<span style="color: #66cc66;">)</span>   
      usage     
      <span style="color: #000066;">exit</span> <span style="color: #cc66cc;">1</span>     
      ;;     
    m<span style="color: #66cc66;">)</span>   
      <span style="color: #0000ff;">MESSAGE=</span><span style="color: #0000ff;">$OPTARG</span>  
      ;;     
    ?<span style="color: #66cc66;">)</span>   
      usage     
      <span style="color: #000066;">exit</span>     
      ;;     
  <span style="color: #b1b100;">esac</span>  
<span style="color: #b1b100;">done</span>  
  
<span style="color: #000066;">shift</span> $<span style="color: #66cc66;">(</span><span style="color: #66cc66;">(</span><span style="color: #0000ff;">$OPTIND</span> <span style="color: #cc66cc;">-1</span><span style="color: #66cc66;">)</span><span style="color: #66cc66;">)</span>  
  
<span style="color: #0000ff;">MESSAGE=</span>$<span style="color: #cc66cc;">1</span>  
  
<span style="color: #b1b100;">function</span> usage <span style="color: #66cc66;">{</span>  
  cat \<\<EOF  
  Usage: $<span style="color: #cc66cc;">0</span> <span style="color: #66cc66;">\[</span>-h<span style="color: #66cc66;">\]</span> <span style="color: #66cc66;">\[</span>-d DOMAIN<span style="color: #66cc66;">\]</span> <span style="color: #66cc66;">\[</span>-e EMAIL<span style="color: #66cc66;">\]</span> <span style="color: #66cc66;">\[</span>-u USER<span style="color: #66cc66;">\]</span>  
  
  This script will post a message to Twitter  
  
  -h            This usage information.  
  -m            Your message  
  -p PASS   Twitter password  
  -u USER       Twitter user name  
   
EOF  
  
<span style="color: #66cc66;">}</span>  
  
<span style="color: #b1b100;">if</span> <span style="color: #66cc66;">\[</span> -z <span style="color: #0000ff;">$USER</span> <span style="color: #66cc66;">\]</span>; <span style="color: #b1b100;">then</span>  
  force\_response <span style="color: #ff0000;">"Enter your Twitter user name: "</span> username  
  <span style="color: #0000ff;">USER=</span><span style="color: #0000ff;">$username</span>  
<span style="color: #b1b100;">fi</span>  
  
<span style="color: #b1b100;">if</span> <span style="color: #66cc66;">\[</span> -z <span style="color: #0000ff;">$PASS</span> <span style="color: #66cc66;">\]</span>; <span style="color: #b1b100;">then</span>  
  force\_response <span style="color: #ff0000;">"Enter your Twitter password: "</span> password  
  <span style="color: #0000ff;">PASS=</span><span style="color: #0000ff;">$password</span>  
<span style="color: #b1b100;">fi</span>  
  
<span style="color: #b1b100;">if</span> <span style="color: #66cc66;">\[</span> -z <span style="color: #ff0000;">"$MESSAGE"</span> <span style="color: #66cc66;">\]</span>; <span style="color: #b1b100;">then</span>  
  force\_response <span style="color: #ff0000;">"Enter your update message: "</span> message  
  <span style="color: #0000ff;">MESSAGE=</span><span style="color: #0000ff;">$message</span>  
<span style="color: #b1b100;">fi</span>  
  
curl -u $<span style="color: #66cc66;">{</span>USER-\`whoami\`<span style="color: #66cc66;">}</span>:<span style="color: #0000ff;">$PASS</span> -d <span style="color: #0000ff;">status=</span><span style="color: #ff0000;">"$MESSAGE"</span> http://twitter.com/statuses/update.xml  
  
<span style="color: #000066;">exit</span> <span style="color: #cc66cc;">0</span>

</div>

</div>

I prefer to copy the script to /usr/local/bin, put my Twitter information in the script itself, and then:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

tweet <span style="color: #ff0000;">'So, today, I totally wrote a bash script and stuff'</span>

</div>

</div>

If you don't like that, you can do something like this:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

./tweet -u username -p password <span style="color: #ff0000;">'DHH and Linus are actually long lost twins.  Discuss.'</span>

</div>

</div>

If you simply execute the script, it should prompt you for the various required information.

I don't claim to be the most skilled bash scripter on the Internets. I encourage you to let me know what you think about it, fork it and improve it, or steal it and tailor it to your use.

Also, I am well aware that I spent more time on this than simplifying that curl call was really worth. I loved every freakin' minute of it, too.

***Note:** Some functions in this script, as well as much of the style, is courtesy of browsing scripts written by my friend [Aaron Schaefer](http://elasticdog.com/). So, thanks, Aaron\!*
