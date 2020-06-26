--- 
layout: post
title: Too Many Choices
date: 2008-07-10 01:54:10 UTC
comments: true
categories: 
--- 
![Stack of Logs](http://soyunperdedor.com/files/nerd_glasses.jpg "I love my nerd glasses!")  
[Photo By Tomotaka](http://www.flickr.com/photos/world9-1/)

I have spent a silly amount of time lately contemplating what I want in a  
blogging engine/system/platform/thing. I know one thing: Drupal, the system I  
am currently using, is not it. It's a fine content management system. In  
fact, I use it as a content management system for another site, and I am quite  
happy with it. However, it's **much** more than what I need. I mean,  
honestly, I inconsistently publish a few thoughts with some code. I do NOT  
need a content management system. So, what do I need?

## WordPress ... ?

Woo\! WordPress\! It's awesome, isn't it? Everyone uses it, so it must be  
good. I have been inside of it, and it seemed nice. It was pretty. It has a  
plugin for everything, I hear. I know that a lot of people use it as their  
content management system. Wow\!

Therein lies the problem. WordPress is *just* big enough to be used as a  
content management system. It does just enough to allow some smaller sites to  
utlitize it to manage their whole site. When I look at WordPress, it still  
seems bigger than my needs. It does too much. It's got a great community.  
I'm sure I could make it do less, or make it do more. It would probably work  
satisfactorily. I don't want to be satisfied, though. I want to be stoked\!  
So, forget WordPress.

## Static Blogging Engines

I am a **huge freakin' nerd**. You know what I want? I want to generate all  
of my blog posts via the command line. I think I'd like to run `rake blog:post FILE=some_file.mkd` to post a new article. I think I  
would like my blog to be completely managed from the command line... or, at  
least, mostly managed from the command line.

I am progressing to this end already. I write my posts in Vim using Markdown  
syntax. Next, I run `markdown blog_post_file_name.mkd`.  
I copy the output, and paste it into the text box in the administration portion  
of my blog. I really would be happy doing it all from the command line. On  
top of all this command-line geekery, the static XHTML pages -- generated from  
such a system -- tend to be fast. Although I don't expect me to ever say  
anything so spectacular, were I to ever get SlashDotted or Digged or Reddited  
or even HackerNewsed, I would not need worry. That is comforting.

**Note**:Having not used the following options extensively to create an actual  
blog, it feels a bit dirty to criticize, but I'm going to do it anyway. You've  
been warned\!

[Hobix](http://hobix.com/ "Hobix Static Blog Engine") is sort of the big name in static blogging, as far as  
Ruby-based systems go. Hobix was initially written by Why the Luck Stiff, and  
powers RedHanded (his abandoned Ruby coding website). Hobix is not too heavily  
developed any longer, from what I can tell. Following the instructions on  
hobix.com for installing Hobix and getting started resulted in an error. It is  
recommended to try the latest development version. I will.

Then there's [Rassmalog](http://snk.rubyforge.org/lib/rassmalog/output/index.html "Rassmalog Static Blog Engine"), which has actually been updated recently.  
(As of this writing, it was last updated June 8th, 2008. The user guide was  
updated on June 22nd, 2008. **Wow\!** Code **and** documentation updates\!  
Very nice\!) It seems to be a well planned solution, and the comprehensive user  
guide is impressive. It offers, initially, email based comments. However,  
Rassmalog's site also describes how to use the JS-Kit comment system. I am  
sure one could use Disqus as well. More on comments and third party comment  
systems later. One final thing to note is that Rassmalog source is available  
via a darcs repository.

Last, but not least, the engine called [Webby](http://webby.rubyforge.org/ "Webby Static Blog Engine"). Not to be confused  
with the Webby awards. Note that the name makes searching for obscure blogs  
that use Webby difficult, as so many people enjoy blogging about the Webby  
awards. In any case, Webby has a few features, it appears, that Rassmalog  
doesn't have, such as LaTeX support, and something about Graphviz. These  
features are also things that I probably wouldn't use. However, I *would*  
potentially use HAML & SASS, as well as Textile, or maybe Markdown, or maybe  
something else. I'm saying it wouldn't be a *bad* thing to have all of these  
options. (Did I not just get done saying I wanted less options? I'm all over  
the place here.) Webby does not have a built in comment system of any sort, but  
people have used Disqus successfully. I'm sure other systems would work fine  
as well. Worth mentioning is that [Webby is on GitHub](http://github.com/TwP/webby/tree/master "Webby on GitHub").

**Update**:  
Oh\! I just stumbled upon [git-blog](http://github.com/elliottcable/git-blog/tree/master "git-blog"). It's so  
geeky, I certainly can't resist at least *trying* it, right? Simple posting.  
Automatic versioning. Git. It is definitely worth a look, for geekery's sake.

## Comments and Comment Systems

I have heard some people question whether comments on blogs are necessary. In  
fact, I can think of at least one well-respected card-carrying Ruby community  
member who eschews blog comments completely. (I am looking at you, [Rein  
Henrich](http://reinh.com/ "Rein Henrich's Site/Blog").) Why? Perhaps spam became unbearable. Maybe he just  
didn't want to deal with comments potentially spiraling out of control and  
becoming a 4chan-like dehumanized zone of ridiculousness. Maybe he gets plenty  
of feedback via IRC, Twitter, et al. There's nothing *wrong* with it. I do,  
however, disagree strongly.

Blog comments are, often enough, the best part of a blog post. How many times  
have you placed a search engine request, visited a blog that seemed to fit your  
keywords well, and found that, indeed, the answer you wanted was actually  
contained in the comments to the original post, rather than in the post itself?  
I love that. It restores one Link-heart worth of faith in humanity and  
community. In addition to that, I often learn something from people commenting  
on my blog posts. I mean, sure, sometimes what I learn is that there's a new  
cream that is guaranteed to make my nights more exciting, but that **is**  

*something*. What about [this comment](http://soyunperdedor.com/node/25 "More Neato Tools blog post") on one of my blog  
posts? Useful. Comments are necessary.

Now that we have established the indispensability of comments, how should they  
be posted? Disqus has become a popular solution to comments. Disqus takes  
much of the burden of dealing with spam, and the other responsibilities of  
managing or writing your own comment system. In exchange? They have your  
data. Oh, and your comments are not going to get indexed by, say, the GOOG,  
because they are dynamically placed in your page via some Javascript. The same  
goes for JS-Kit, and probably others in that niche. I want that data, and I  
want it indexed. How are people to find the helpful comments? I am sure you  
have a retort readily prepared, dying to flow from your fingertips. Please,  
feel free to leave a comment. However, the bottom line is that it doesn't sit  
well with me. So, man-in-the-middle comment system? Just say no\!

(A couple interesting links related to blogs and comments from people I respect: [Tim Bray just before he had a commenting system](http://www.tbray.org/ongoing/When/200x/2006/05/20/Comments-on-Comments), and [Sam Ruby's advice to Tim Bray on a commenting system](http://www.intertwingly.net/blog/2006/05/15/Comments-Please). Enjoy.)

I will figure out a way to manage my own comments. This will probably mean comment content ultimately ending up in my inbox before they are made public. I will decide how spammy they are, and then I'll  
post them. (Hopefully by typing 'rake comment --FILE=some\_file' or something.) There will need to be some initial filters in place, of course. Anyway, we'll see.

## So...

I'm looking at Rassmalog and Webby, mostly. I think I might end up going with  
Webby, and gluing some sort of hacked together comment script on to the bottom  
of the thing. It should last for a while. Maybe a PHP script. Or, a Ruby  
script maybe? Will I need a little Rack-based app?

Is this what you would do? Are you perfectly happy with WordPress, and you  
think I'm insane (unsurprised, on both counts)? Let me know. Comment.  
Please?
