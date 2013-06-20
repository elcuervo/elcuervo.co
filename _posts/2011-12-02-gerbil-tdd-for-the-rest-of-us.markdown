---
layout: post
title: Gerbil, TDD for the rest of us
published: true
categories: [tdd, js]
hashtags: gerbil, tdd, js
---
![Gerbil](/posts_assets/gerbil-header.png)

Gerbil is my attempt to use a really simple (for real) tdd framework
in js, both browser and node.js.
The main idea is to provide a simple way to test certain behaviours like assert,
assertEqual, assertThrow, etc.
Currently the API in really simple and will eventually include some cool
features.

Most of the time the tests that we need to execute are not THAT complex, one of
my side project is [Lodis](http://github.com/elcuervo/lodis) an implementation
in javascript of the well known [Redis Server](http://redis.io).

For it i'm using LocalStorage as foundation to implement the Redis commands, for
that i needed a way to test things in the browser. But all of it seemed so
complex for a simple console.log.

So i started that, a REALLY SIMPLE testing suite to do that, assert to
console.log. Eventually i've needed some other things so it grow enough to
release it.


## Install

You can see the project in [Github](http://github.com/elcuervo/gerbil)

Using [npm](http://npmjs.org/)

{% highlight bash %}
$ npm install gerbil
{% endhighlight %}

Or in a browser

{% highlight html %}
https://raw.github.com/elcuervo/gerbil/master/build/gerbil.pack.js
{% endhighlight %}

## A simple example

{% highlight javascript %}
scenario("This is my scenario", {
  "setup":  function() {
    // When scenario starts
    this.someThing = new Thing;
  },
  "before": function() {
    // Before every test
    this.someThing.magic_magic();
  },
  "after":  function() {
    // After every test
    this.someThing.clean();
  },
  "cleanup": function() {
    // When the scenario ends
    this.someThing = false;
  },

  "MagicClass should have a length": function(g){
    this.someThing.add(1);
    g.assertEqual(this.someThing.length, 1);
  },

  "MagicClass should be valid": function(g) {
    g.assert(this.someThing.valid);
  }
});
{% endhighlight %}

## Example output
### Console

As i said node.js is supported so the output will look like this:

![Console Output](/posts_assets/gerbil-console-output.png)
![Console Output](/posts_assets/gerbil-error-output.png)

### Browser

On the other hand if you ran it within a browser by default the output will be
like this:

![Browser Output](/posts_assets/gerbil-browser-output.png)

### Custom

But somethimes you just want to show some thing in your tests so there is a way
to use a custom logger to show your tests any way you want.

{% highlight javascript %}
var my_cool_logger = {
  "warn":   function(msg){},
  "log":    function(msg){},
  "info":   function(msg){},
  "error":  function(msg){
    alert(msg);
  },
};

scenario("Fancy scenario", {
  "somewhere over the rainbow": function(g){
    g.assert(false);
  }
}, my_cool_logger);
{% endhighlight %}

![Custom Logger](/posts_assets/gerbil-custom-logger.png)
