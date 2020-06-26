---
layout: post
title: "Belated Emacs Update"
date: 2013-01-01 08:20
comments: true
categories: programming emacs
---

You might remember that, last March, [I had been using Emacs regularly (as well as Vim)](http://mileszs.github.com/recapping-29-days-with-emacs/), and was planning to install [Evil]("http://emacswiki.org/emacs/Evil"), a Vim emulator in Emacs. Well, I did that. I then used Emacs almost exclusively for around seven months. Me, a **Vim guy**, used Emacs for **seven months**!

I am not going to wade too deep analyzing my likes and dislikes, this time.

## I Like Emacs

I do really like Emacs. Aside from [all of the reasons mentioned previously](http://mileszs.github.com/recapping-29-days-with-emacs/), installing Evil mode made Emacs like a Vim++, almost. It was really great, except for when it wasn't. There were a couple accidental key combinations that would kill me. For instance, some key combination causes Emacs to ping a server whose address it derives from the word or sentence string under the cursor if it involves a dot. Also, indentation in Ruby was never quite right, and Javascript syntax highlighting and indentation was never right, ever.

All of those things are fixable. I could've remapped keys, overridden functions, tied in to other plugins using their hooks, and all using Elisp, which is somewhat pleasant. I did, in some cases. It was fun, actually, learning a little bit of Elisp, and bending Emacs a bit. Ultimately, I found my way back to Vim.

## Home Again

To explain to you how I found my way back, I'll tell you how I work right now: I pop open iTerm, change into my project's directory (which I should automate), and type `work [projectname]`. This opens or re-attaches a tmux session with several panes and windows, each running a utility -- a server, REPL, log tail, and, of course, a Vim session.

In other words, I've regressed, in a way, to how I worked us GNU Screen 3 years ago. Tmux is a big reason why I'm back using Vim.

One other reason is simplicity. I tend toward (or intend to tend toward) simplicity in many ways in life. Emacs is great, but it's certainly not simple. It isn't a goal for Emacs to be simple, nor should it be.

Emacs is great. You should absolutely try it for a while. It's just not for me.
