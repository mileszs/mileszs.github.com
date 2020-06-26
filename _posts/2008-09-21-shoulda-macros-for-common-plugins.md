--- 
layout: post
title: Shoulda Macros for Common Plugins
date: 2008-09-21 04:12:35 UTC
comments: true
categories: 
--- 
If you haven't looked into [Shoulda](http://thoughtbot.com/projects/shoulda), maybe you should. It's a plugin (and now gem) that works "on top of" Test::Unit. It truly does make it "easy to write elegant, understandable, and maintainable tests".

The following are a few Shoulda macros that I use for the plugins **acts\_as\_ferret**, **acts\_as\_taggable\_on\_steroids**, and **acts\_as\_list**. I recommend creating a file 'test/shoulda\_macros/plugins.rb', and placing these macros in there. For example:

    class Test::Unit::TestCase
    
    # Shoulda macros here
    
    end

## Acts As Ferret

      def self.should_act_as_ferret(*fields)
        klass = self.name.gsub(/Test$/, '').constantize
    
        should "include ActsAsFerret methods" do
          assert klass.extended_by.include?(ActsAsFerret::ClassMethods)
          assert klass.include?(ActsAsFerret::InstanceMethods)
          assert klass.include?(ActsAsFerret::MoreLikeThis::InstanceMethods)
          assert klass.include?(ActsAsFerret::ResultAttributes)
        end
    
        fields.each do |f|
          should "create an index for field named #{f}" do
            assert klass.aaf_configuration[:fields].include?(f)
          end
        end
      end

Use it like:

    should_act_as_ferret :any, :fields, :i_may, :have, :specified

## Acts As Taggable On Steroids

      def self.should_act_as_taggable_on_steroids
        klass = self.name.gsub(/Test$/, '').constantize
    
        should "include ActsAsTaggableOnSteroids methods" do
          assert klass.extended_by.include?(ActiveRecord::Acts::Taggable::ClassMethods)
          assert klass.extended_by.include?(ActiveRecord::Acts::Taggable::SingletonMethods)
          assert klass.include?(ActiveRecord::Acts::Taggable::InstanceMethods)
        end
    
        should_have_many :taggings, :tags
      end

## Acts As List

      def self.should_act_as_list
        klass = self.name.gsub(/Test$/, '').constantize
    
        context "To support acts_as_list" do
          should_have_db_column('position', :type => :integer)
        end
    
        should "include ActsAsList methods" do
          assert klass.include?(ActiveRecord::Acts::List::InstanceMethods)
        end
    
        should_have_instance_methods :acts_as_list_class, :position_column, :scope_condition
      end

**Note:** These tests are quite basic. Except for checking the fields on **acts\_as\_ferret**, there isn't any testing of each plugin's options.

These macros are available at <http://github.com/mileszs/shoulda_macros>. You can find more macros at the Shoulda wiki on GitHub, <http://github.com/thoughtbot/shoulda/wikis/example-macros>. If you write some of your own shoulda macros, you should link to them on the wiki as well.
