---
layout: post
title: "Using Zsh to Rename in Rails"
date: 2011-12-03 5:59
categories: programming
---

At the [Indy.rb Code Review Session](http://www.meetup.com/indyrb/events/40640362/) (11-30-11), [Dan Doezema<](http://dan.doezema.com) asked if there was an easy way to rename a _resource_>, or the related model, controller, view directory, tests, and potentially factory or fixture. We had a good laugh after someone mentioned that being one of the features that lure people to try Smalltalk, or, perhaps, the Ruby VM [MagLev](http://maglev.github.com/). The joke is that doing either of those two things is very likely a road to compromised mental health.

## A Christmas Miracle!

This renaming issue was on still bouncing around inside my mind as I began reading the first day of [this ZSH-focused advent calendar](http://www.refining-linux.org/categories/13/Advent-calendar-2011/). Hey, look at that! Between day one and day two, one can piece together a nice line for renaming the files related to a resource in Rails.

```
$ cd project_root
$ zmv '(**/)old_name(*)' '$1new_name$2'
$ git status
# On branch master
# Changes not staged for commit:
#   (use "git add/rm ..." to update what will be committed)
#   (use "git checkout -- ..." to discard changes in working directory)
#
#   deleted:    app/controllers/old_names_controller.rb
#   deleted:    app/models/old_name.rb
#   deleted:    app/views/old_names/new.html.haml
#   deleted:    test/unit/old_name_test.rb
#
# Untracked files:
#   (use "git add ..." to include in what will be committed)
#
#   app/controllers/new_namess_controller.rb
#   app/models/new_name.rb
#   app/views/new_names/
#   test/unit/new_name_test.rb
no changes added to commit (use "git add" and/or "git commit -a")
```

## Santa’s Little Helper

That is only _half_ of the problem, though. In fact, it’s arguably the easier-solved of the two halves, whether you use Bash, ZSH, or something completely different. How are we going to edit the content of the files? After the above command, we’re still stuck with this:

```
$ vim app/models/new_name.rb
class OldName
```

More command-line work to the rescue!

```
$ git ls-files --other --exclude-standard | xargs sed -i '' 's/OldName/NewName/'
$ vim app/models/old_name.rb
class NewName

Great! Two lines of terminal work and we’ve reached our goal!

## Good Enough

This isn’t perfect. You still might have to change other references to the class, such as a <code>has_many :old_items</code> line in another class, or something similar. If you have a better method you’ve been employing (short of installing an IDE), I would love to hear it!
