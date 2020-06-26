--- 
layout: post
title: Setting Up a New Slice of Ubuntu 7
date: 2008-01-06 15:37:13 UTC
comments: true
categories: 
--- 
Today, I decided to rebuild one of my slices hosted with [SliceHost](http://slicehost.com/) (because [SliceHost Rocks\!](http://soyunperdedor.com/?q=node/5)). It was really doing anything, and I have decided I want to hack on a few projects in which this slice may come in handy. (More on those to come, possibly.)

### Psshh, This is NOT New

There are several other blogs, wikis, and how-to sites that present all of the information I am going to present here, and probably more. In fact, the first place to stop is [SliceHost's Wiki](http://wiki.slicehost.com/doku.php). However, putting this all in once place could still be useful, even if it is only useful for me in a few months when I go to do this again. I intend to add some explanation of the steps, as well. Everyone likes a little explanation. It's delicious\!

### Rebuild Your Slice

Rebuilding a slice is the easiest part. Seriously. You just click the 'Rebuild' link (so intuitive\!) and you are presented with this screen:

![Screenshot of Slicehost Manager rebuild screen](http://soyunperdedor.comfiles/slicehost_rebuild.png "It's simple to rebuild a slice with this interface")

Choose an operating system. Choices include Arch, CentOS, Fedora, Gentoo, Debian, and both Ubuntu 6.06 and 7.10. Ubuntu 6.06 is the long-term support version; in other words, it is considered very, very stable. Ubuntu 7.10 is the newest release of Ubuntu, and is the operating system that I chose here.

Now choose a name for your slice, preferably related to Futurama in some way.

After making your choices and clicking the rebuild button, Slicehost will tell you what your root password is, so pay attention.

### Initial Setup

First, we need to add a user that will be our primary administrative user. You'll want to SSH to your slice, like:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

ssh root@\<IP-of-your-slice\>

</div>

</div>

Add the user.

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

adduser --ingroup users \<username\>

</div>

</div>

Now make sure this user is a sudoer. (This command opens up /etc/sudoers in the default editor, which will probably be nano.)

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

visudo

</div>

</div>

At the end of the file, insert this line:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

\<username\> <span style="color: #0000ff;">ALL=</span><span style="color: #66cc66;">(</span>ALL<span style="color: #66cc66;">)</span> ALL

</div>

</div>

Now, press Ctrl and 'O', Enter, then Ctrl and 'X'. In other words, save the file and exit nano.

We want to disallow the ability to SSH into this machine via the 'root' account, from now on. I won't get into exactly why, just trust me.

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

vim /etc/ssh/sshd\_config

</div>

</div>

Find the 'PermitRootLogin' line and change 'yes' to 'no'. Then, reload SSH:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

/etc/init.d/ssh reload

</div>

</div>

There are other things you can do to secure SSH. Some people even recommend changing the port used. If you want more information, ['Hardening a Linux or OpenBSD Installation'](http://www.cromwell-intl.com/security/linux-hardening.html) is recommended reading.

At this point, I would exit the SSH session, and re-login as the user you just created. So, uh, go do that, already. Also, I personally like to add my slice's IP to my /etc/hosts file (if you're using some version of Linux as your desktop) on my local computer, so that I can just 'ssh my-name@slice-name'. Simple.

### Can We Install Crap Now?

We are now going to install some stuff. Woo hoo\! We are going to install updates\! Hey, quit moaning. You\! Quit calling me that\!

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sudo apt-get update  
sudo apt-get upgrade

</div>

</div>

Now we're going to install a couple packages so we can quit getting warnings about our locale settings.

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sudo apt-get install localization-config language-pack-en

</div>

</div>

You could install a lot of stuff, if you want. I'm going to install the build-essentials package, which comes with compilers, and all of the good stuff you need to install stuff from source. I think I'm going to go ahead and install NTP packages, to help the server keep the correct time. I also like to install the vim-full package, in case I have to do much direct editing on my slice (for instance, freakin' Apache configuration files).

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sudo apt-get install build-essential ntp ntpdate vim-full

</div>

</div>

This could take some time. I recommend going to get a Mountain Dew from the fountain station of your local convenience store. I like to call these Fountain Dews. Don't you wish you were as cool as I am?

### Installing Ruby

Alright, alright. Now we'll install fun stuff, like Ruby, Ruby Gems, . Luckily for us, Ubuntu now has a 'ruby-full' package that installs Ruby version 1.8.6, and nearly everything you might want with it. (In particular: ruby, irb, rdoc, ri, libdbm-ruby, libgdbm-ruby, libreadline-ruby, libopenssl-ruby, ruby1.8-dev.) To quote the package's description:

> "For many good reasons, the Ruby programming language is split in many  
> small different packages. Installing this package will make sure you have  
> all the packages that add up to a full Ruby installation, with the exception  
> of the Tcl/Tk bindings for Ruby which are only recommended.  
> .  
> This package installs the dependencies for the current stable  
> version of Ruby, which is 1.8."

So...

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

apt-get install ruby-full

</div>

</div>

If you want to try Ruby 1.9, you'll have to look elsewhere. I'm sorry. Ruby 1.8.6 continues to be the most stable version, although I do intend to play with Ruby 1.9 sooner than later.

I'm sure you noticed that RubyGems were not installed in that package. You don't want the outdated version that is in the Ubuntu repositories (currently 0.9.4). You want the latest, greatest version. RubyGems 1.0.1\!

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

wget http://rubyforge.org/frs/download.php/<span style="color: #cc66cc;">29548</span>/rubygems<span style="color: #cc66cc;">-1.0</span><span style="color: #cc66cc;">.1</span>.tgz  
tar xvzf rubygems<span style="color: #cc66cc;">-1.0</span><span style="color: #cc66cc;">.1</span>.tgz  
<span style="color: #000066;">cd</span> rubygems<span style="color: #cc66cc;">-1.0</span><span style="color: #cc66cc;">.1</span>  
sudo ruby setup.rb

</div>

</div>

Sweet\! That was easy. Right? Now we can install Rails version 2.0.2\!

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sudo gem install rails

</div>

</div>

**Note:**If the above command results in a `sudo: gem: command not found` message, you will need to set a symbolic link from /usr/bin/gem1.8 to /usr/bin/gem. You could also copy 'gem1.8' to 'gem'.

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sudo ln -s /usr/bin/gem1<span style="color: #cc66cc;">.8</span> /usr/bin/gem

</div>

</div>

Gems now automatically accepts dependencies. If you want to ignore any dependencies, there's an option for that.

### That's All Folks\! ... for now

I am going to stop right here, for now. You've got Ruby, RubyGems, and Rails, on top of Ubuntu Gutsy. I would say that's the basics. In the future, I do intend to cover getting a Rails app deployed, although, you can find that a million other places, also. I do hope this helps someone, somewhere. Let me know if it does\!
