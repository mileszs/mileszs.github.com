--- 
layout: post
title: More Neato Tools
date: 2008-05-29 01:36:52 UTC
comments: true
categories: 
--- 
I am very late to most parties. I usually bring beer, though I am a beer snob. If you prefer some light American lager, you're out of luck. So, I'm late and I bring beer that few people like (and often times, it's just me). This metaphor will make a ton of sense by the end of this post.

## Sake Party\!

[Sake](http://errtheblog.com/posts/60-sake-bomb) is short for System-wide Rake. It was created by Chris at Err The Blog (now of GitHub fame\!). Sake is awesome.

How did I miss most of this party?

At its core, Sake is a set of commands to work with rake tasks that are saved in a file called '.sake' in your home directory. You can install tasks from \*.rake files, or from plain text files. You can install tasks that are local, or remote. You can run these tasks from anywhere in your system using the 'sake' command instead of 'rake'. Suddenly, rake becomes easily usable for system task scripting.

## Party with Pastie\!

It's [Pastie](http://pastie.caboo.se/). You know Pastie, right? It's a place to paste code for sharing. It has syntax highlighting and stuff. It's awesome.

## Merge the parties\!

I mention Pastie so that I can talk about Sake and Pastie combined. You can browse and install Sake tasks from Pastie\! Do this:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sake -T http://pastie.caboo.se/<span style="color: #cc66cc;">205143</span>.txt

</div>

</div>

You will get a task list of some of the tasks that I have in my Sake file. Simply add '.txt' to the end of your typical Pastie URL, and you'll get the pastie in plain text. What is even more awesome is how I got all my tasks to Pastie:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sake -P | xsel

</div>

</div>

The fragment 'sake -P' sends all of your tasks to Pastie and returns the URL of your new pastie. You could do 'sake -P one\_task' if you only wanted to send one of your tasks there. That sake command is piped to the command 'xsel', which, in Ubuntu (and likely other Linux flavors) will copy the URL to your clipboard. You'll need to install xsel first, though.

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sudo apt-get install xsel

</div>

</div>

In OSX, you can pipe it to 'pbcopy', which is already installed.

Remember when you listed tasks from a pastie of mine? Let's install one of those.

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sake -i http://pastie.caboo.se/<span style="color: #cc66cc;">205143</span>.txt pastie:send

</div>

</div>

Sweet\! Sake went ahead and put that code in your \~/.sake file. I use this sake task frequently. The task you just installed is courtesy of [Dr. Nic](http://drnicwilliams.com/2007/06/28/pastie-paradise/), and it's awesome. I use it often, like this:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

cat \~/devel/scratch | sake pastie:send | xsel

</div>

</div>

Now I can send some quick code examples, diffs, or obscene ASCII art to my boss, and it is syntax-highlighted\!

## That's awesome\! I thought you brought beer no one wants?

If you list the tasks from that pastie again, you'll notice several tasks that either already exist in the current version of Rails, or are nearly obsolete because they deal with Subversion. (It's nearly dead, you know. At least, that's what everyone tells me.) See? Crap no one wants to drink.
