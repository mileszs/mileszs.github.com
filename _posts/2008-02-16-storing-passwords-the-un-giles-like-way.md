--- 
layout: post
title: Storing Passwords the Un-Giles-Like Way
date: 2008-02-16 03:13:46 UTC
comments: true
categories: 
--- 
Giles Bowkett wrote an entry in [his blog](http://gilesbowkett.blogspot.com/) today, introducing his Ruby gem 'password'. (The article is [sudo gem install password](http://gilesbowkett.blogspot.com/2008/02/sudo-gem-install-password.html).) As you may have guessed, the gem allows you to store user names and passwords that you might otherwise forget. Unfortunately, all it does is take them and throw them in a text file, in plain text. Mr. Bowkett goes on to explain his philosophy about passwords and web applications. It can best be summarized by the sentence, "I don't give a fuck." (No, really. Go read it, it's a decent read. These are his words.)

## Why is this a big deal?

I am not going to preach about the dangers of saving passwords in plain text. It's obvious to everyone, and it is more than obvious to Mr. Bowkett. He makes his position clear. In fact, the focus of his article was not even his 'password' gem, really. His focus was registration requirements for websites and why they suck.

The reason I am referencing the article is because of that gem, though. I'm sure it's a perfectly wonderful gem, with some perfectly wonderful code. Unfortunately, it's completely unnecessary. There is a perfectly good command-line utility called 'pwsafe' that stores your *encrypted* passwords in a database for you. I'll mention it again: **encrypted**.

If you're on Ubuntu or a derivative:

<div class="codeblock">

<div class="bash" style="font-family: monospace;">

sudo apt-get install pwsafe

</div>

</div>

It's pretty straightforward. You can have groups of user name-passowrd combinations. You can set a password (separate from your OS user account) for the database. You can set it up so that it requires you to use 'sudo' to run the program. Eventually, you can just ssh into your box from anywhere, enter your credentials, and get that damned [Remember The Milk](http://www.rememberthemilk.com/) password that you keep forgetting. (Forgetting that particular user name and password makes keeping track of your task list a bit difficult, by the way. I don't remember how to work this 'pen and paper' ridiculousness.)

## Cool\! How secure is that?

I am sure a security expert could come here and school me on the actual security of using pwsafe. I am not an expert. However, it's certainly better than plain text in a plain file. Far better.

## Do you actually use it?

I have been using the program for quite a while, and I love it. I like using 'pwgen' to generate random passwordsfor different things that I have to register (Yes, Giles, it does suck). I can't remember all of them. Hell, I can't freakin' remember what I had for lunch yesterday. Throw them in here with a reasonable reference name (uh, such as, 'rememberthemilk.com'), and you're good to go.
