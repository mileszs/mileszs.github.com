--- 
layout: post
title: Vim + rails.vim Seg-Faults in Ubuntu 8
date: 2008-04-29 18:30:00 UTC
comments: true
categories: 
--- 
Like many have, I installed the latest version of Ubuntu late last week. I did not have a lot of time to get Hardy Heron ready for day-to-day development until Monday. While using it for half a day yesterday, Vim had been producing segmentation faults from time to time. It was irritating, but wasn't happening often enough to get me too excited to track down the issue.

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

Vim: Caught deadly signal SEGV

</div>

</div>

That is, until today. It's been absolutely infuriating. I'm a guy that might, you know, punch or throw an inanimate object in moments of frustration. Thus far, I have managed to avoid damaging anything, but it's been close.

Thankfully, I am not the only one with this problem. Here is the [Ubuntu bug report](https://bugs.launchpad.net/ubuntu/+source/vim/+bug/219546), bug \#219546. To summarize, the plugin rails.vim, in concert with Vim version 7.1.138, seg-faults when using tab completion together with a rails.vim command such as ':RTcontroller pe\[TAB\]\[TAB\]'.

### Crap. What do I do?

Within the bug report, a few have suggested that installing from source will fix the issue -- or, install from the Debian Sid packages. I chose to install from source. That seemed to fix the issue for me.

A quote from dominiko, within the bug report comments:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

If I download the latest vim <span style="color: #66cc66;">(</span>vim<span style="color: #cc66cc;">-7.1</span><span style="color: #cc66cc;">.293</span><span style="color: #66cc66;">)</span> <span style="color: #000066;">source</span> code <span style="color: #66cc66;">(</span>see  
http://www.vim.org/download.php<span style="color: #66cc66;">)</span>, and compiled it myself:  
  
  <span style="color: #000066;">cd</span> vim7  
  ./configure --with-<span style="color: #0000ff;">features=</span>huge  
  make  
  make install  
  
Then it works fine...

</div>

</div>

That is my recommendation. If you have any trouble, you are welcome to comment here and I will do what I can to help. Be wary\! Be warned\! Take my advice at your own risk\! I am **far** from an expert.
