---
layout: post
title: "Combining FactoryGirl's Generated Files Into One"
date: 2011-12-04 5:59
categories: programming
---

If you are using [Thoughtbot’s FactoryGirl](\"https://github.com/thoughtbot/factory_girl_rails\") in a
Rails 3 app, you are probably accustomed to the library generating a new
file per factory/model it generates. You end up with something like
this:

```
$ ls test/factories
things.rb other_things.rb more_things.rb omg_things.rb
```

I really dislike it generating a file per factory. I prefer one file in
which I can see each factory I will be using in my tests. I am,
evidently, the odd man out. I dug around the Interwebs a bit,
unsuccessfully. This led to me fixing it myself witha Rake task:

``` ruby
namespace :factories do
  desc 'Combine individual factory files into a single file'
  task :combine do
    File.open(File.join(Rails.root, 'test', 'factories.rb'), 'a') do |f|
      Dir.foreach(File.join(Rails.root, 'test', 'factories')) do |factory|
        next if factory == '.' or factory == '..'
        full_path = File.join(Rails.root, 'test', 'factories', factory)
        text = File.read(full_path)
        f.write(text)
        puts "Removing #{full_path}..."
        File.delete(full_path)
      end
    end
    puts "Removing the factories directory..."
    Dir.rmdir(File.join(Rails.root, 'test', 'factories'))
    print "Done."
  end
end
```

I run that whenever I have generated a new model or resource (and thus a
new factory file).
