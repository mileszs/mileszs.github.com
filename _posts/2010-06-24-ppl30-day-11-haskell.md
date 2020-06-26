---
layout: post
title: "PPL30 Day 11: Haskell"
date: 2010-06-24 21:33:22 -0400
comments: true
categories: [ppl30, programming]
---
These sessions are smoother when I string together similar technologies. Thus, today was Haskell, nipping the heels of yesterday's Erlang. Haskell strikes me as Erlang with a different syntax and an advanced type system. Should I duck now? In any case, I'd describe both of them as **Functional as Fuck**&#0153;.

<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

!["Eddie Haskell"](/images/eddie-haskell.jpg "Hi! I'm Haskell! Why are you running away?")

## Installation ##

In a bit of a twist, you must install 'ghc' (Glasgow Haskell Compiler -- I know, I expected GNU, too!) instead of 'haskell'. That's the most interesting thing this section has seen in a while.

    brew install ghc

## Resources ##

### Learn You a Haskell for Great Good ###

As I mentioned yesterday, [Learn You a Haskell](http://learnyouahaskell.com) is a free online book inspired (somewhat) by [Why's Poignant Guide to Ruby](http://mislav.uniqpath.com/poignant-guide/). It's good, too. It is the book that acted as a model for [Learn You Some Erlang](http://learnyousomeerlang.com/). (If you read them back to back, as I have, it's plain that much of the content of [Learn You Some Erlang](http://learnyousomeerlang.com/) is taken straight from [Learn You a Haskell](http://learnyouahaskell.com) (with permission), and modified just enough to fit Erlang.) Again, this was the only resource I used, because it was enjoyable. Why waste time searching when I could be learning?


## What I Think I Now Know ##

Haskell is ... interesting. As I mentioned, it is statically typed, but it has type inference. You don't have to specify the type of everything, as the compiler will figure it out. However, it is considered good practice to state the type for function parameters and return values explicitly. Of course, every expression must return something (this is sort of a requirement in functional programming), and it must have a specific type. Those two statements merge to create brick walls against which you will bash your head, eventually. 

The typing system is different and intriguing, though, mostly due to typeclasses. You could have a function which takes one parameter of any type, and returns that type. This is a polymorphic function. That can further be constrained by typeclasses. Using typeclasses, you could have a function which takes one parameter of any type, and returns that type, but additionally that type must be a member of a specific typeclass. Typeclasses shouldn't be mistaken as classic OO classes, but rather more along the lines of a simple interface definition. For instance, if a type is a member of the typeclass `Eq`, then that typeclass supports equality testing. You'll notice in my FizzBuzz the function `show`, which is essentially converting the type of a value to a type that is a member of the `Show` typeclass.

Shake your head back and forth, really fast, three times. Okay, moving on.

Another interesting note about Haskell: it uses prefix function notation, typically. You can force infix notation if the function takes exactly two parameters (by surrounding the function name with backticks), and functions comprised of only special characters (+, -, *, ==, etc.) are infix by default. 

Yesterday, in my Erlang FizzBuzz, I wrote:

<% coderay(:lang => :erlang, :line_numbers => :inline) do #_ -%>
  -module(fizzbuzz).
  -export([fizz/3, doit/0]).

  fizz(0, 0, Num) ->
    io:format("FizzBuzz~n");
  fizz(0, _, Num) ->
    io:format("Fizz~n");
  fizz(_, 0, Num) ->
    io:format("Buzz~n");
  fizz(_, _, Num) ->
    io:format("~B~n", [Num]).

  doit() ->
    Go = fun(El) -> fizz(El rem 3, El rem 5, El) end,
    L = lists:seq(1,100),
    lists:foreach(Go, L).
<% end -%>

Joel Meador of [Expected Behavior](http://expectedbehavior.com) suggested a couple alternatives:

<% coderay(:lang => :erlang, :line_numbers => :inline) do #_ -%>

  %% First, partial suggestion
  doit() -> 
  [fizz(X rem 3, X rem 5, X) || X <- lists:seq(1,100)]. 
   
  %% Second, full re-write
  -module(fizzbuzz). 
  -export([dostuff/0, fizzbuzz/1]). 
   
  fizzbuzz(X) when X rem 3 == 0, X rem 5 == 0 -> "fizzbuzz"; 
  fizzbuzz(X) when X rem 3 == 0 -> "fizz"; 
  fizzbuzz(X) when X rem 5 == 0 -> "buzz"; 
  fizzbuzz(X) -> X. 
   
  dostuff() -> 
  P = fun(X) -> io:format("~p~n",[X]) end, 
  [P(fizzbuzz(X)) || X <- lists:seq(1,100)]. 

<% end -%>

Well, this inspired me to take better advantage of the language features of Haskell, especially when I discovered it shares a lot of concepts, and even syntax, with Erlang. See for yourself:

<% coderay(:lang => :erlang, :line_numbers => :inline) do #_ -%>
  fizz :: (Num t, Num t1, Num t2) => t -> t1 -> t2 -> [Char]
  fizz 0 0 _ = "FizzBuzz"
  fizz 0 _ _ = "Fizz"
  fizz _ 0 _ = "Buzz"
  fizz _ _ x = show x

  doit = [show (fizz (mod x 3) (mod x 5) x) | x <- [1..100]]

  main = sequence_ [putStrLn x | x <- doit]
<% end -%>

I won't go into as much detail as I did yesterday, as the two are pretty similar. A couple things of note: the line beginning `fizz ::` is the type declaration for the `fizz` function. I left it off for the other two functions, allowing the compiler to figure it out. Also, `main` is more-or-less like `main` in a C program. 

I won't be surprised if Joel shows up again to suggest an even better solution. I'm learning, though. (What I'm trying to say is: thanks, Joel, and other commentors. Keep it coming!)

So, Haskell is interesting. It seems more academic than practical, as do many things when you've not put in the time required to understand them. I did get a bit tired of fighting the type system, even in my FizzBuzz implementation! However, I think everyone should learn them some Erlang and a Haskell. It's great exercise for the brain.

* * *

Tomorrow? [Factor](http://factorcode.org/), baby. Erlang and Haskell didn't fry my brain quite enough, evidently. 
