--- 
layout: post
title: Re-Thinking Hack-Svn && Ship-Svn
date: 2008-09-09 17:22:39 UTC
comments: true
categories: 
--- 
In a comment to [Hack-svn && Ship-svn](http://soyunperdedor.com/node/31), [Yossef Mendelssohn of OG Consulting](http://bl.ogtastic.com/), suggested that my chosen workflow may not be ideal. He probably knows much better than I. Here's the (relevent portion of the) comment:

> Also, about your git-svn scripts. Kevin (the third OG) was working on the same thing, even going so far as to make 'hack' and 'ship' DTRT based on whether you're in a git-svn repo or a regular git repo. In my experience, however, there's more difference between vanilla git and git-svn than just whether you git pull or git svn rebase. I've had enough troubles that I no longer dcommit from master. I always keep master as the state of the svn repo and dcommit from branches. Much fewer headaches that way.

*(Note that the three OGs are Yossef Mendelssohn, Rick Bradley, and Kevin Barnes.)*

I had previously seen a similar idea suggested by a commentor to Andy Delcambre's ['Git SVN Workflow'](http://andy.delcambre.com/2008/03/04/git-svn-workflow.html) by the name of Chris McGrath:

> That workflow is very similar to how I use git-svn. Something I don’t do that I’ve seen in nearly every article like this is the merge back to master. I just git-svn rebase my topic branch to svn HEAD, fix any conflicts there and git-svn dcommit that, possibly doing a rebase -i beforehand to clean up the commit(s). Then I switch to master, rebase again to get the committed changes and delete the topic branch. My reasons for this aren’t really biggies imho, more that this is the way I’ve always done it. They are:
> 
> 1.  Master will always contain only changes that are in svn, so easy to make a quick bugfix branch without having to work out which rev to branch from. 2. The git-svn man page doesn’t recommend doing git merges, but using dcommit or format patch to move changes between git branches. 3. It’s one less place to get conflicts, as they will only happen on the topic branch rebase. ...

Fair enough.

## So, that means what?

Well, following the basics of Yossef's suggestion would make ship-svn really, really boring. (That's not to say it is incorrect, or ineffective.)

    #!/bin/sh -x
    CURRENT=`git name-rev HEAD --name-only`
    git svn dcommit

Right? Should we also checkout master and 'git svn rebase' again? That step would get done the next time you run 'hack-svn' anyway. Hmm. Maybe there is another, better workflow of which I am unaware?

Mr. McGrath is suggesting something like:

    #!/bin/sh -x
    CURRENT=`git name-rev HEAD --name-only`
    git svn rebase
    git svn dcommit
    git checkout master
    git svn rebase
    git branch -D ${CURRENT}

I don't really like that. However, I do have a nasty habit of working out of the same 'topic' branch constantly, rather than naming a new one with each new feature. Sometimes I do feel the need to have a few different branches active, if I am unsure of the best path, and I intend to discover it through experimentation. Most of the time, I just work out of 'devel'. That approach makes the adaptation of the original Hack && Ship method pleasing, to me.

Again, that probably means I'm wrong. I will likely adopt my interpretation of Yossef's method soon, while sticking with the original for now. I'd like to think about it for a bit. I would also enjoy hearing what others think.
