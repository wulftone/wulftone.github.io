---
layout: post
title: Uglifier changes constructor names
date: 07/08/2013
categories: coffeescript uglifier javascript
---

## Gotcha!... again.

So, it turns out that relying on the names of your constructors being the same in production as they are in development is a really bad idea.  The reason is this: *minifiers change all that crap!*  I know what you're saying, "Duh, they're supposed to, dufus."  Okay yeah so it slipped my mind for a second.  Good thing I didn't deploy without testing first!  ; )

Specifically, I was writing a little ditty in CoffeeScript in Ruby on Rails and was getting some strange behavior only when I started up a production server.  Data were getting inexplicably mangled beyond recognition, and I couldn't find the source of the problem... after all, it worked great in development mode!  So without further ado, here's what happened:

Here's a couple Backbone models:

{% highlight coffeescript %}
class H.Models.User extends Backbone.Model

class H.Models.Household extends Backbone.Model
{% endhighlight %}

Here's the constructors for those two models, after being compiled into javascript:

{% highlight javascript %}
a = new H.Models.User
User {_queue: Backbone.BlockingQueue, cid: "c33", attributes: Object, _changing: false, _previousAttributes: Object…}

a.constructor
function User() {
  _ref = User.__super__.constructor.apply(this, arguments);
  return _ref;
}

b = new H.Models.Household
Household {_queue: Backbone.BlockingQueue, cid: "c47", attributes: Object, _changing: false, _previousAttributes: Object…}

b.constructor
function Household() {
  _ref = Household.__super__.constructor.apply(this, arguments);
  return _ref;
}
{% endhighlight %}

The names of the two constructors are `User` and `Household`.  Makes sense, right?  But the constructors get mangled into the same thing by the Uglifier gem:

{% highlight javascript %}
// Dev Tools console
a = new H.Models.User
n {_queue: n.BlockingQueue, cid: "c7", attributes: Object, _changing: false, _previousAttributes: Object…}

a.constructor
function n() {
  return t = n.__super__.constructor.apply(this, arguments);
}

b = new H.Models.Household
n {_queue: n.BlockingQueue, cid: "c33", attributes: Object, _changing: false, _previousAttributes: Object…}

b.constructor
function n() {
  return t = n.__super__.constructor.apply(this, arguments);
}
{% endhighlight %}

Notice `a.constructor.name === b.constructor.name`.  Oops.  So yeah... don't rely on them being `User` or whatever in any code.  ; )
