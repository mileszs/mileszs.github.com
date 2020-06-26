---
layout: post
title: "Deprecation Warnings for proxy_owner in Rails 3.1"
date: 2011-07-21 5:59
categories: programming
---

In Rails, you are used to doing something like this:

``` ruby
has_many :assignments do
  def root
    proxy_owner.assignments.where(:ancestry => nil).first
  end
end
```

This way, if you call `parent_record.assignments.root`, the root method
acts on the array of assignments that belong to that parent. It’s nice.
However, in Rails 3.1 you will see a big scary message in your logs, or
in your test output.


```
DEPRECATION WARNING: Calling record.assignments.proxy_owner is deprecated. Please use record.association(:assignments).owner instead. (...)
```

I will admit that it is not very frightening, actually. It is mostly
merely annoying… annoying **as hell**. Get out of my test output!

The working solution that I found looks like this:


``` ruby
has_many :assignments do
  def root
    @association.owner.assignments.where(:ancestry => nil).first
  end
end
```

This functionality is now defined in [collection_proxy.rb]("https://github.com/rails/rails/blob/v3.1.0.rc4/activerecord/lib/active_record/associations/collection_proxy.rb"), which was quite a bit of help in finding this solution. Bid those deprecation warnings *adieu*!

PS – *I do not know whether this is the core-team accepted solution. I
am certainly happy to see a better one!*
