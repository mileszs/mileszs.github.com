---
layout: post
title: "PPL30 Day 20: MooTools"
date: 2010-07-03 10:12:41 -0400
comments: true
categories: [ppl30, programming]
---
MooTools is a Javascript framework that I have heard combines the object-oriented, programmer-focused philosophy seen in Prototype.js with the ease of jQuery when working with the DOM. There are those who dislike jQuery because it's not made for programmers. 

![happy burger cow](/images/happy-burger-cow.jpg "This is a giant cow sculpture outside a restaurant in a town very near where I grew up. I'm not joking.")

<em><small>This is part of the Peer Pressure Learning 30 series. To learn more about what in the hell that means, see the intro to the series, [A Good Time To Learn](http://mileszs.com/blog/2010/06/13/a-good-time-to-learn.html).</small></em>

## Resources ##

### MooTools Docs ###

[The MooTools docs](http://mootools.net/docs/core) are pretty good, though I think they could use more examples. If you are familiar with both jQuery and Prototype.js, I recommend just digging into the docs to get started.

### The MooTorial ###

The guide to which most are pointed is called [The Mooturial](http://www.mootorial.com/), which makes me giggle a bit (and is making [Elijah Miller](http://jqr.github.com/) cringe. I guarantee it). I list it here mostly because it seems to be the common recommendation. It should be noted that I mostly read the docs or Googled for random snippets.

### jQuery vs. MooTools ###

[A slightly biased, but mostly fair look at how jQuery compares to MooTools](http://jqueryvsmootools.com/) was written by Aaron Newton, and author of a book about MooTools. I recommend checking it out. See jQuery beside well-written MooTools code is quite useful and informative.

## What I Think I Now Know ##

To the point, MooTools feels very similar to Prototype.js. It should, as they share much of the same philosophy; they both are aiming to enhance Javascript, as opposed to create a toolkit for working with the DOM (which is what jQuery is). That philosophy really pleases the part of me that we'll call **Theoretical Programmer Miles**. MooTools and Prototype.js are both _frameworks_ written for _programmers_. They have a lot of interesting native extensions to Javascript, a take on inheritance, etc. 

However, **Practical Programmer Miles** wants to get things done. When I'm writing client-side javascript, _most_ of the time I want to manipulate the DOM in some way. That is why jQuery has become so popular; jQuery makes jobs focused on manipulating the DOM quick and easy. It makes **Practical Programmer Miles** happy.

Let's look at a quick example before we go any further. Here is a function (that could probably be rectored) for showing the remaining characters in an input (think: Twitter) in jQuery:

<% coderay(:lang => :javascript, :line_numbers => :inline) do -%>
$(document).ready(function() {
  if ($("#suggestion_body").length) {
    $("li#suggestion_body_input .inline-hints").text(140 - $("#suggestion_body").val().length);
    $("#suggestion_body").keypress(function(e) {
      $("li#suggestion_body_input .inline-hints").text(140 - $(this).val().length);
    });
  }
});
<% end %>

Here is the same function in MooTools:

<% coderay(:lang => :javascript, :line_numbers => :inline) do -%>
// MooTools
window.addEvent('domready', function() {
  if ($defined($('suggestion_body'))) {
    $('suggestion_body').addEvent('keyup', function() {
      current_length = $('suggestion_body').value.length; 
      remaining_chars = 140 - current_length;
      $$('li#suggestion_body_input .inline-hints')[0].set('html', remaining_chars);
    });
  }
});
<% end %>

Man, they're not very different. They shouldn't be, really -- they're both javascript. They might be more similar than you expected. I'm going to steal a couple examples from [jQuery vs. MooTools](http://jqueryvsmootools.com/):

<% coderay(:lang => :javascript, :line_numbers => :inline) do -%>
// jQuery
$(document).ready(function() {
    $("#orderedlist li:last").hover(function() {
        $(this).addClass("green");
    },
    function() {
        $(this).removeClass("green");
    });
});

// MooTools 
window.addEvent('domready',function() {
    $$('#orderedlist li:last-child').addEvents({
        mouseenter: function() {
            this.addClass('green');
        },
        mouseleave: function() {
            this.removeClass('green');
        }
    });
});
<% end %>

Is the MooTools version more explicit? Absolutely. It's also more verbose as a result. The above example succinctly illustrates the difference between jQuery and MooTools, and a reason why I'll continue to use jQuery. This may not seem like a big deal, but imagine an increase in verbosity across all javascript on your site. I like Javascript (I never thought I'd say that), and I often like working with Javascript, but I think that most websites benefit from keeping their Javascript to as few lines as possible while still doing the cool things they want to do. jQuery helps there. At least with Prototype, the lines of code seem to balloon very quickly.

Do I think there's no place for Prototype and MooTools? NO! I really like the things they've done. In larger, Javascript heavy projects, the Javascript language extensions and inheritance stuff can be very useful. That said, I'd still want jQuery for the DOM stuff!

I should have focused on Prototype versus MooTools in this post. The two are so similar that I think such an article would be a bit more interesting. The differences between jQuery and MooTools have not just been covered already, but are also quite obvious. So... I'm sorry. 

![sorry puppy](/images/sorry-puppy.jpg "Sorry Puppy. I'm trying to promote puppy pictures. I hate cats.")
