---
layout: post
title: A Git Hook to Push to Integrity
date: 2009-03-03 20:14:53 -0500
comments: true
categories:
---
If you’re using [GitHub](http://github.com) with [Integrity](http://integrityapp.com), “the easy and fun automated continuous integration server”, then you probably know how stupid-simple it is to integrate the two. Seriously. You just tell GitHub to what URL you wish it to send stuff, and it’ll do it. It takes more time for most computers to power up than it takes to get that setup running. Great, isn’t it?

If you’re hosting your own centralized git repository on your own server, you need to set up your own post-receive hook. There is a [short thread](http://groups.google.com/group/integrityapp/browse_thread/thread/895f3b6198275f6e) about doing just that at [Integrity’s Google Group](http://groups.google.com/group/integrityapp). In fact, that is where our journey begins…

## Could someone spell lazy for me?

![](/images/caturday-lazyday.jpg)

I knew that I needed a post-receive hook that sent the right information to my installation of Integrity, but I really didn’t want to have to write one myself. Sure, it might be fun. It also might be a frustrating waste of time. So, I went to the Integrity Google Group, searched for “post receive”, and came across [this thread](http://group) s.google.com/group/integrityapp/browse\_thread/thread/895f3b6198275f6e (the same one to which I linked above). Cool\! That led to [this Gist](https://gist.github.com/669b761f47e61ec665ad) by whatcould:

*Watch out for that wayward ’0’ on line 33 in the Gist.*

```ruby
#!/usr/bin/ruby

#   1.  Replace the following by the url given to you in your project’s edit page
POST_RECEIVE_URL = 'http://localhost:4567/my_fancy_project/push'

#################################################################
## DON'T EDIT ANYTHING BELOW UNLESS YOU KNOW WHAT YOU'RE DOING ##
#################################################################


require ‘net/http’
require ‘uri’

old_head, new_head, ref = STDIN.gets.split

# 1.  on solaris, ruby is choking on various shell commands, the quotes in format.
# 2.  maybe it’s running sh? (though it seems to report bash)?
# 3.  the `bash -c …` gets around it, anyhow:

revisions = %x{bash -c "git-rev-list —pretty=format:'%H %ai' #{new_head} ^#{old_head}"}.split("\n")

rev_list = revisions.inject([]) do |list,line|
  if line[0..5] == 'commit'
    list
  else
    list << %Q({ "id": "#{line[0..40]}", "timestamp": "#{line[41..66]}" })
  end
end

Net::HTTP.post_form(URI.parse(POST_RECEIVE_URL), {
  :payload => %Q({"ref":"#{ref}", "commits":[#{rev_list.join(',')}]})
})
```

## Hurry Up\!

You can just copy and paste that script to `GIT_REPO_ROOT/hooks/post-receive`, `chmod +x GIT_REPO_ROOT/hooks/post-receive`, and you should be good to go. The next time that you `git push`, it’ll push the necessary information to Integrity. Eventually. It takes a while. The waiting was driving me nuts. One of the great things about git is how freakin’ fast *everything* is. This script was killing me. Perhaps I’m just really impatient. Maybe I’m doing something wrong. I have a solution, though.

## And Then There Was Awesome

We’re going to use the “Daemons” gem to make this freakin’ fast. OK, technically, it doesn’t speed anything up. It just gets out of the way. Whatever. Do this:

```
sudo gem install daemons
```

Now we just have to add a couple lines to the post-receive hook:

_This version is available as a gist [here](https://gist.github.com/4d81e30bbc4aac556dec)._

```
#!/usr/bin/ruby
# Git post-receive hook for pushing to Integrity

# Replace the following by the url given to you in your project's edit page
POST_RECEIVE_URL = 'http://localhost:4567/my_fancy_project/push'

#######################################################################
##                                                                   ##
## == DON'T EDIT ANYTHING BELOW UNLESS YOU KNOW WHAT YOU'RE DOING == ##
##                                                                   ##
#######################################################################

require 'net/http'
require 'uri'
require 'rubygems'
require 'daemons'

old_head, new_head, ref = STDIN.gets.split

# on solaris, ruby is choking on various shell commands, the quotes in format.
# maybe it's running sh? (though it seems to report bash)?
# the `bash -c ...` gets around it, anyhow:


revisions = %x{bash -c "git-rev-list --pretty=format:'%H %ai' #{new_head} ^#{old_head}"}.split("\n")           

rev_list = revisions.inject([]) do |list,line|
  if line[0..5] == 'commit'
    list
  else
    list << %Q({"id":"#{line[0..40]}","timestamp":"#{line[41..66]}"})
  end
end

Daemons.daemonize

Net::HTTP.post_form(URI.parse(POST_RECEIVE_URL), {
  :payload => %Q({"ref":"#{ref}", "commits":[#{rev_list.join(',')}]})
})
```

`Daemons.daemonize` will shove our currently-running script to the background and let git get on with gittin’. It’s simple. It’s fast. Now, it’s **legendary**\!
<br />

![](/images/legendary-barney.jpg)

_Image borrowed from [SoutherDesigner at DeviantArt](http://southerndesigner.deviantart.com/art/Legendary-Barney-Stinson-102733576)_
