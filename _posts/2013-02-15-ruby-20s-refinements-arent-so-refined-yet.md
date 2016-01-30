---
layout: post
title: Ruby 2.0's refinements aren't so refined... yet?
date: 15/02/2013
categories: ruby
---

I began fiddling with the cool new features in [Ruby 2.0.0-rc2](http://www.ruby-lang.org/en/news/2013/02/08/ruby-2-0-0-rc2-is-released/) and quickly ran into some strangeness:

{% highlight ruby %}
# refine_test.rb
module Doubler
  refine Fixnum do
    def double
      self * 2
    end
  end
end

using Doubler
class Thing
  def initialize
    p 2.double
  end
end

# using Doubler #=> putting this here would result in
# line 10 producing a NoMethodError for `2.double`
Thing.new  # => 4
p 2.double  # => 4
load 'refine_load_file_test.rb'
{% endhighlight %}

And the other file:

{% highlight ruby %}
# refine_load_file_test.rb
Thing.new  # => 4
p 2.double  # => NoMethodError
{% endhighlight %}

The `load` command runs `Thing.new` and returns a `4` just peachy... however the next line fails.  [Ruby-doc](http://www.ruby-doc.org/core-1.9.3/Kernel.html#method-i-load) doesn't give any clues as to why this would work like that.
