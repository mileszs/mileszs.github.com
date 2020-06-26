---
layout: post
title: "Checking All Checkboxes with Capybara"
date: 2010-08-19 5:59
categories: programming
---

This could be useful to someone searching the Interwebs for a solution.
Hopefully it will save someone some time, as it took some time to figure
it out. Looking back, I can see where someone might find it obvious. I
didn’t, for some reason.

The following is how I accomplished it, using a CSS selector:

``` ruby
When /^I choose all of the damn checkboxes$/i do
  all('li.checkboxes input').each {|ch| check(ch[:id]) }
end
```

This requires that the checkbox `input` elements have a unique `id`
attribute. You might, having some knowledge of Capybara’s `Element`
class, think that you can just use `#path`, which returns an elements
XPath string, in place of `[:id]`. That’s what I thought, and I welcome
you to try it — what kind of hackers would we be if we believed it every
time someone said something wasn’t possible? I, unfortunately, was
unable to make it work. The `check` method reported that it could not
find an element with said `path` (or XPath).

I’d love to hear it if you have better luck!
