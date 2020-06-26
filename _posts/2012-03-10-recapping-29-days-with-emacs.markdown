---
layout: post
title: "Recapping 29 Days with Emacs"
date: 2012-03-10 5:59
categories: programming
---

Before the beginning of the month of February, I endeavored to make
February 29 days of exclusively using Emacs to edit code. I wanted to
fully immerse myself in Emacs, and force myself to learn its ways. Why?
I wanted to understand why people love it, including several friends. I
wanted to truly know how it compares to my favorite editor, Vim. I
wanted to make myself learn something new.

As you probably guessed, I was unable to *exclusively* use Emacs for the
entire 29 days; I had to open Vim a few times when I felt like I was
losing productivity at a time when I could not afford to be losing
productivity. This really wasn’t a surprise. I knew trying to stop Vim
“cold-turkey”, and replace it with Emacs, would leave me sweating,
twitching, and pining for Vim.

That said, Emacs certainly has features that I liked.

Pros before Noes
----------------

### Self-bootstrapping

I tell Emacs what packages I want to definitely be there, and it ensures
those packages are present using [ELPA]("http://tromey.com/elpa/")
and, in my case, [Marmalade]("http://marmalade-repo.org/"). The
necessary lines in my config look like this:

``` common-lisp
    (require 'package)
    (add-to-list 'package-archives
      '("marmalade" . "http://marmalade-repo.org/packages/") t)
    (package-initialize)

    (when (not package-archive-contents)
      (package-refresh-contents))

    (defvar my-packages '(starter-kit starter-kit-ruby starter-kit-js js2-mode haml-mode php-mode css-mode markdown-mode)
      "A list of packages to ensure are installed at launch")

    (dolist (p my-packages)
      (when (not (package-installed-p p))
      (package-install p)))
```

### Superior File Finding

Emacs' “default” method of searching for files is superior to Vim’s. Its
auto-complete is better, its matching is better (slightly fuzzy?), and
it seems faster. I hate navigating to files using `:e` in Vim. It is my
least favorite aspect of Vim. (I also hate project drawer plugins, and
PeepOpen is too slow, and not integrated enough, for my tastes. Let me
know if there’s another plugin that I should use.) There’s a plugin for
Emacs, [textmate.el]("https://github.com/defunkt/textmate.el"), that
further enhances this (though it uses a different shortcut by default)
by restricting it to the project you’re in. I sometimes like this, and
sometimes I don’t. Thankfully, I can use both. The plugin textmale.el
adds a few other things I like, actually. I was surprised by it. Of
course, Chris Wanstrath wrote it, so my surprise was naivety, or maybe
just stupidity.

### Syntax Checking

This is a bit weird for me. I mean, I would normally argue that I don’t
need syntax checking, I don’t need an IDE, I don’t need all that bloat.
I would still *normally* make that argument. However, February wasn’t
normal. I have been doing a lot of PHP work. Too much PHP work. Well,
any PHP work is too much, but you get the point. PHP makes me stupid,
somehow. I end up writing awful code. I end up missing parens and
single-quotes and double-quotes and brackets and semi-colons and — if
there is a bit of required syntax in PHP, you can bet I’ll figure out a
way to forget it or fuck it up. Someone should study me as I write PHP,
then study me writing Ruby or Javascript, and then explain to me and the
world what about PHP makes me imbecilic.

Using [flymake-php]("http://www.emacswiki.org/emacs/FlymakePhp"), I
was able to cut down on those stupid mistakes. I’m not sure it’s always
useful, but it was useful in this case.

I actually wrote some elisp to run CoffeeScript through CoffeeLint upon
a particular key-chord. It’s not very good, my code, and it isn’t using
Flymake. However, it’s a testament to …

### Elisp/Extensibility

Elisp is better than VimL, in my limited experience, but I think even
people who write a lot of VimL would agree there. Emacs is fairly
obviously more extensible than Vim. To be fair, Emacs and Vim have
different philosophies — Vim doesn’t *want* to do all the stuff that
Emacs does. Because I dig the idea of simplicity/minimalism/focusing on
a few things and doing them really well, I feel some kinship with Vim.
In practice, I own a lot of stuff I don’t need, and I try to do far too
many things to truly excel at any single one. There’s a good chance
Emacs and I are actually more alike.

Noes
----

### Paper Cuts

“Paper cuts”, as in “death by a thousand”, is the best label for the
bucket that holds all the small complaints I have about using Emacs.
Individually, they aren’t significant; they are but a small cut on a
finger tip. Collectively, holy shit balls, my hands hurt. Getting Ruby
indentation the way I want it was a pain. Actually, getting to a
specific line in a file annoys the piss out of me every single time I do
it, and I do it a lot. Perhaps most people don’t navigate files the way
I do, but `M-g M-g 65 RET` is infuriating when compared to `65G`.

I think [Ross Baker said it best when he said]("https://twitter.com/#!/rossabaker/status/172493698834235393"): “Emacs: Hard things made possible. Easy things oftentimes made hard.”

That sums up Emacs' primary negative, for me. It does leave me with the
feeling that I can get Emacs to a point of perfection if I keep trying,
though.

### Lacking Modal Editing

Of course, `M-g M-g 65 RET` is merely a side-effect of Emacs not doing
modal-editing out of the box. Vim is *the* modal editor, and that is all
it does. Naturally, it is magnificent, if you like that sort of thing.
Guess what? I do. I really, really do. Using Emacs (in lieu of Vim) for
29 days did more to convince me that modal editing is the most efficient
way to edit text than several years of Vim use. When I completed a task
quickly with Vim, after getting frustrated trying to get things done
with Emacs, I felt like I needed to smoke a cigarette. Oh yeah,
modal-editing. Mmm, yeah.

So, It’s Over?
--------------

I really have been using Emacs every day. In fact, it’s open as I type
this, with some code from a client’s Rails project in the open buffer. I
intend to continue using it in March, even though I can let myself off
the hook. However, I do intend to install and use
[Evil]("http://emacswiki.org/emacs/Evil"), which grants some
modal-editing abilities. I had barred myself from that crutch up until
now. I will let you know what I think of it.

I also intend to write a post, at the risk of inciting the next battle
in the war, with my view of Emacs versus Vim. Preview: they’re both
great.
