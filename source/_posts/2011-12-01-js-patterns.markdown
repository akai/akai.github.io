---
layout: post
title: "JavaScript Design Patterns"
date: 2011-12-01 14:16
comments: true
categories: javascript
---

Writing code that’s expressive, encapsulated & structured

1. Module Pattern
-----------------

Interchangeable single-parts of a larger system that can be easily re-used.

### Stepping stone: IIFE

Immediately invoked function expressions (or self-executing anonymous functions)

{% codeblock lang:js %}
(function() {
    // code to be immediately invoked
}()); // Crockford recommends this way

(function() {
    // code to be immediately invoked
})(); // This is just as valid

(function(window, document, undefined) {
    //code to be immediately invoked
})(this, this.document);

(function(global, undefined) {
    //code to be immediately invoked
})(this);

{% endcodeblock %}

### Simulate privacy

The typical module pattern is where immediately invoked function expressions (IIFEs) use **execution context** to create ‘privacy’. Here, **objects** are returned instead of functions.

{% codeblock lang:js %}
var basketModule = (function() {
    var basket = []; //private
    return { //exposed to public
        addItem: function(values) {
            basket.push(values);
        },
        getItemCount: function() {
            return basket.length;
        },
        getTotal: function() {
            var q = this.getItemCount(), p = 0;
            while (q--) {
                p += basket[q].price;
            }
            return p;
        }
    }
}());
{% endcodeblock %}

- In the pattern, variables declared are only available inside the module.
- Variables de ned within the returning object are available to everyone
- This allows us to simulate privacy

### Sample usage

Inside the module, you'll notice we return an object. This gets automatically assigned to basketModule so that you can interact with it as follows:

{% codeblock lang:js %}
//basketModule is an object with properties which can also be methodsbas
basketModule.addItem({item: 'bread',price: 0.5});
basketModule.addItem({item: 'butter',price: 0.3});

console.log(basketModule.getItemCount());
console.log(basketModule.getTotal());

//however, the following will not work:
// (undefined as not inside the returned object)
console.log(basketModule.basket);
//(only exists within the module scope)
console.log(basket);
{% endcodeblock %}

### Module Pattern: jQuery

In the following example, a **library** function is defined which declares a new library and automatically binds up the **init** function to document.ready when new libraries (ie. modules) are created.

{% codeblock lang:js %}
function library(module) {
    $(function() {
        if (module.init) {
            module.init();
        }
    });
    return module;
}

var myLibrary = library(function() {
    return {
        init: function() {
        /*implementation*/
        }
    };
}());
{% endcodeblock %}

### AMD + jQuery

A very simple AMD module using jQuery and the color plugin

{% codeblock lang:js %}
define(["jquery", "jquery.color", "jquery.bounce"], function($) {
    //the jquery.color and jquery.bounce plugins have been loaded
    // not exposed to other modules
    $(function() {
        $('#container')
            .animate({'backgroundColor': '#ccc'}, 500)
            .bounce();
    });
    // exposed to other modules
    return function () {
        // your modules functionality
    };
});
{% endcodeblock %}

### AMD/UMD jQuery Plugin

**Recommended** by Require.js author James Burke

{% codeblock lang:js %}
(function(root, factory) {
    if (typeof exports === 'object') {
        // Node/CommonJS
        factory(require('jquery'));
    } else if (typeof define === 'function' && define.amd) {
        // AMD. Use a named plugin in case this
        // file is loaded outside an AMD loader,
        // but an AMD loader lives on the page.
        define('myPlugin', ['jquery'], factory);
    } else {
        // Browser globals
        factory(root.jQuery);
    }
}(this, function($) {
    $.fn.myPlugin = function() {
    };
}));
{% endcodeblock %}

### CommonJS Modules

They basically contain two parts: an **exports** object that contains the objects a module wishes to expose and a **require** function that modules can use to import the exports of other modules

{% codeblock lang:js %}
/* here we achieve compatibility with AMD and CommonJS
using some boilerplate around the CommonJS module format*/
(function(define){
    define(function(require,exports){
        /*module contents*/
        var dep1 = require("foo");
        var dep2 = require("bar");
         exports.hello = function(){...};
         exports.world = function(){...};
    });
})(typeof define == "function" ? define : function(factory) { factory(require, exports) });
{% endcodeblock %}

2. Facade Pattern
-----------------

Convenient, high-level interfaces to larger bodies of code that hide underlying complexity

### Facade Implementation

A higher-level facade is provided to our underlying module, without directly exposing methods.

{% codeblock lang:js %}
var module = (function() {
    var _private = {
        i: 5,
        get: function() {
            console.log('current value:' + this.i);
        },
        set: function(val) {
            this.i = val;
        },
        run: function() {
            console.log('running');
        },
        jump: function() {
            console.log('jumping');
        }};
    return {
        facade: function(args) {
            _private.set(args.val);
            _private.get();
            if (args.run) {
                _private.run();
            }
        }}
}());
module.facade({run: true,val: 10}); //outputs current value: 10, running
{% endcodeblock %}

3. Mediator Pattern
-------------------

Encapsulates how disparate modules interact with each other by acting as an intermediary

### Mediator Implementation

One possible implementation, exposing publish and subscribe capabilities.

{% codeblock lang:js %}
var mediator = (function() {
    var subscribe = function(channel, fn) {
        if (!mediator.channels[channel]) mediator.channels[channel] = [];
        mediator.channels[channel].push({ context: this, callback: fn });
        return this;
    },

    publish = function(channel) {
        if (!mediator.channels[channel]) return false;
        var args = Array.prototype.slice.call(arguments, 1);
        for (var i = 0, l = mediator.channels[channel].length; i < l; i++) {
            var subscription = mediator.channels[channel][i];
            subscription.callback.apply(subscription.context, args);
        }
        return this;
    };

    return {
        channels: {},
        publish: publish,
        subscribe: subscribe,
        installTo: function(obj) {
            obj.subscribe = subscribe;
            obj.publish = publish;
        }
    };
}());
{% endcodeblock %}

### Example

Usage of the implementation from the last slide.

{% codeblock lang:js %}
//Pub/sub on a centralized mediator

mediator.name = "tim";
mediator.subscribe('nameChange', function(arg) {
    console.log(this.name);
    this.name = arg;
    console.log(this.name);
});

mediator.publish('nameChange', 'david'); //tim, david

//Pub/sub via third party mediator

var obj = {name: 'sam'};
mediator.installTo(obj);
obj.subscribe('nameChange', function(arg) {
    console.log(this.name);
    this.name = arg;
    console.log(this.name);
});

obj.publish('nameChange', 'john'); //sam, john
{% endcodeblock %}

*Form http://www.slideshare.net/AddyOsmani/futureproofing-your-javascript-apps-compact-edition*
