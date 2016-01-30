---
layout: post
title: "Rspec Gotchas: before, after, all, and each"
date: 22/01/2012
categories: ruby rspec
---

I decided to make a simple rspec like the ones you find in the [rspec docs](https://www.relishapp.com/rspec) describing the leftovers in the [before and after hooks documentation](https://www.relishapp.com/rspec/rspec-core/docs/hooks/before-and-after-hooks) that they never discussed.  I ran into issues in my project, because it's not necessarily totally intuitive what order things are happening when you get into a lot of befores and afters.

Here it is:

{% highlight ruby %}
describe "before and after callbacks" do

  before(:all) do
    puts 'outer before all'
  end

  before do
    puts 'outer before each'
  end

  it "outer group description" do
    puts 'running outer group spec'
  end

  describe "describing nested group" do

    before(:all) do
      puts '  inner before all'
    end

    before do
      puts '  inner before each'
    end

    it "nested group description" do
      puts '  running nested group spec'
    end

    after do
      puts '  inner after each'
    end

    after(:all) do
      puts '  inner after all'
    end
  end

  after do
    puts 'outer after each'
  end

  after(:all) do
    puts 'outer after all'
  end
end
{% endhighlight %}

Here's what happens when we run the spec:

{% highlight bash %}
$ rspec --format documentation

before and after callbacks
outer before all
outer before each
running outer group spec
outer after each
  outer group description
  describing nested group
  inner before all
outer before each
  inner before each
  running nested group spec
  inner after each
outer after each
    nested group description
  inner after all
outer after all

Finished in 0.00059 seconds
2 examples, 0 failures
{% endhighlight %}

As you can see, the intersting bit is in the nested `describe` group of tests, where it first does the *inner* before all, then it does the *outer* before each before doing the *inner* before each.  Weird!  So be careful not to get zapped by any outer before each block if you want to do an inner before all in a nested group of tests.  Also notice when the descriptions get printed vs. when the spec actually runs.

Let's see if it continues the trend when we nest another level:

{% highlight bash %}
$ rspec --format documentation

before and after callbacks
outer before all
outer before each
running outer group spec
outer after each
  outer group description
  describing nested group
  inner before all
outer before each
  inner before each
  running nested group spec
  inner after each
outer after each
    nested group description
    describing doubly-nested group
    doubly-inner before all
outer before each
  inner before each
    doubly-inner before each
    running doubly-nested group spec
    doubly-inner after each
  inner after each
outer after each
      doubly-nested group description
    doubly-inner after all
  inner after all
outer after all

Finished in 0.00083 seconds
3 examples, 0 failures
{% endhighlight %}

Looks like it does, so there you have it!
