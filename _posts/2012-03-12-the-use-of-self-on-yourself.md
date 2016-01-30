---
layout: post
title: "How to change the value of an object: Ruby bang! methods you can implement yourself"
date: 12/03/2012
categories: ruby
---

I was having trouble finding answers about how to implement my own bang methods in Ruby, so I figured some things out and consolidated what little info there is here.  Bang! methods are methods that end with an exclamation point--this is only a convention, not a language construct--and simply mean "Hey! Pay attention to what you're doing here!", but more often than not, they modifiy the calling object's value... so for instance, like this:

{% highlight ruby %}
a = 'foo'
a.gsub!('foo','bar') # => 'bar'
a # => 'bar'
{% endhighlight %}

The value of `a` has changed to `'bar'`.  The string `'foo'` is gone.  Reminder, *not all* bang methods change their calling object in this way--but many of them do.  Do how do we make our own?  Most of them are implemented in the low level code (Ruby MRI is written in C) using pointers and whatever.  We don't have access to that kind of thing in Ruby, so what do we do?  Let's find out what our options are.

We'll use the good 'ol Ruby `#next` method to have some fun with our`selves`:

{% highlight ruby %}
a = 'foo'
a.next # => 'fop'
a # => 'foo'

b = :foo
b.next # => :fop
b # => :foo
{% endhighlight %}

So that's what next does.  Easy.  Now, let's make a bang! method so we can "commit" those changes to the calling object:

{% highlight ruby %}
class String
  def nextify!
    self.length.times do |a|
      self[a] = self[a].next
    end
    self
  end
end

a = 'foo' # => 'foo'
a.nextify! # => 'gpp'
a # => 'gpp'
{% endhighlight %}

Neat!  We can also do this to an `Array`:

{% highlight ruby %}
class Array
  def nextify!
    self.length.times do |a|
      self[a] = self[a].next if self[a].respond_to? :next
    end
    self
  end
end

a = [1,'a',:a] # => [1,'a',:a]
a.nextify! # => [2,'b',:b]
a # => [2,'b',:b]
{% endhighlight %}

What will you do with your own bang methods?  Are there any other ways to make these happen in pure Ruby?  If so, let me know!
