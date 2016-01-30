---
layout: post
title: 'Backbone: dynamically change view events upon initialization'
date: 16/05/2013
categories: backbone javascript
---

Just for people who might find this, you can modify the events object in your `initialize` method like so:

```js
var MyView = Backbone.View.extend({

  events: {
    'click .foo': 'handleClickFoo'
  },

  initialize: function(options) {

    // Add an event
    if ( /* something */ ) {
      this.events['click .save'] = function() {
        //do stuff
      };
    }

    // Remove an event
    if ( /* something else */ )
      delete this.events['click .foo'];
  }

});
```

`delegateEvents` will get called after initialize, and your events hash will be dynamically changed by all the fiddling you do in the `initialize` method.
