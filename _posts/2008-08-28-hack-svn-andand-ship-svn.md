--- 
layout: post
title: Hack-Svn && Ship-Svn
date: 2008-08-28 21:16:18 UTC
comments: true
categories: 
--- 
I was inspired today by [Rein's Hack && Ship post](http://reinh.com/blog/2008/08/27/hack-and-and-ship.html "Rein Henrich's Hack && Ship post") to write something similar for those of us stuck in the **Dark Ages of Git-SVN**. It took very little work on my part. Honestly, the originals did most of the work. I changed *two lines*.

What I'm trying to say is: All praise be to the original author, Rick Bradley, and to Rein for exposing them to the **Rest of Us**. ***Edit:***Thanks also to Yossef Mendelssohn for the idea and his tips in the comments section, and to Kevin Barnes, because he's part of that crew surely had a hand in it -- and he's working on a sweet related script. Again, see Yossef's comment.

## Hack-svn && Ship-svn: The Scripts

**Hack-svn**:

    #!/bin/sh -x
    CURRENT=`git name-rev HEAD --name-only`
    git checkout master
    git svn rebase
    git checkout ${CURRENT}
    git rebase master

**Ship-svn**:

    #!/bin/sh -x
    CURRENT=`git name-rev HEAD --name-only`
    git checkout master
    git merge ${CURRENT}
    git svn dcommit
    git checkout ${CURRENT}

## You really are un perdedor

As you can see, I indeed only changed two lines to make them work for me. The names 'hack-svn' and 'ship-svn' are not nearly as fun as 'hack' and 'ship', whether reading them aloud, or typing them after completing a feature. However, I want to also have vanilla-git hack and ship available for vanilla-git projects. The name at least makes their function semi-obvious, if you know about hack && ship. I thought about just calling them 'hacks' and 'ships', giving them a bit of a Paris Hilton feel. Loves it?

Yes, I obsess over what to call scripts I write. It's a disease. When I recover, I'll start some sort of rehab clinic bearing my name.

So, have at it, my fellow git-svn-ers. You might also write a local alias for git svn rebase and git svn dcommit to make you feel like you aren't really using subversion. It's therepeutic.
