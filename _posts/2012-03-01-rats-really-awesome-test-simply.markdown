---
layout: post
title: RATS, Really Awesome Tests Simply
published: true
assets: rats
categories: [javascript, tdd, simple]
hashtags: js, tdd
tags: [js, tdd, cubox]

---

There is always a simpler way of doing things, i love to do things as
simple as posible. But sometimes you need tools for that, easy and simple tools
and sometimes thoose tools does not exists. That's why i've wrote a couple of
tools to make my Javascript development easier.

## Gerbil
[https://github.com/elcuervo/gerbil](https://github.com/elcuervo/gerbil)

![Gerbil](/images/posts/rats/gerbil.jpg)

I've covered Gerbil
[before](/tdd/js/2011/12/02/gerbil-tdd-for-the-rest-of-us.html) but there are
some updates and new users are always welcome.


Gerbil was created with the purpose of doing javascript testing a really simple
and no-dependecy thing. It can run in a browser or within nodejs.

    {% highlight javascript %}
      // Name the scenario you want to test and pass an object with your tests.
      scenario("Some usefull stuff that needs to work", {
        // Reserved names are 'setup', 'before', 'after' and 'cleanup'. They define
        // the steps to be executed.
        //
        // Every test gets one parameter, this is the test suite itself.
        // Modifying 'this' will affect the context in the tests, it's useful when
        // using 'setup' to initialize some state.
        'setup': function(g) {
          this.validName = 'Gerbil';
        },
        // Within the test 'this' gets the config defined in 'setup'
        'should get the correct name': function(g) {
          g.assertEqual(this.validName, 'Gerbil');
        },

        // Test in the feature, usefull to test future events or timers.
        'in the future': function(g) {
          this.time = new Date().getTime();

          g.setTimeout(function() {
            g.assert(new Date().getTime() > this.time);
          }, 1000);
        },

        // Test async code.
        //
        // Using the async function you can control the status of the test. This is
        // really usefull when you are testing callbacks.
        // But remember, it's your responsability to end() the test.
        'should be able to test asyncronous code': function(g) {
          var asyncStuff = function() {
            this.callback = null;
          };

          asyncStuff.prototype = {
            eventually: function(fn) {
              this.callback = fn;
            },

            exec: function() {
              setTimeout(function(c) {
                c.callback();
              }, 500, this);
            }
          };

          g.async(function() {
            var async = new asyncStuff;
            async.eventually(function() {
              g.assert(true);
              // end() will end the current scenario and trigger a summary
              g.end();
            });
            async.exec();
          });
        }

      });
    {% endhighlight %}

## Smoking
[https://github.com/elcuervo/smoking](https://github.com/elcuervo/smoking)

![Smoking](/images/posts/rats/smoking.jpg)

Smoking it's a tool to mock and stub objects in javacript it was originally
created to prevent some code execution during some tests but afterwards i've
realize that it can be used for method interception and testing. So i've
abstracted it in a npm package.

As Gerbil it has no dependencies.

### Stubbing

    {% highlight javascript %}
      var Fruit = function(color) {
        this.color = color;
        this.healthy = 'yes';
      };

      Fruit.prototype = {
        cutInPieces: function() {
          return Math.floor(Math.random()*11);
        }
      };

      var redFruit = new Fruit('red');

      redFruit.color;
      // 'red'
      redFruit.healthy;
      // 'yes'
      redFruit.cutInPieces();
      // 5

      var stubbedRedFruit = smoking(redFruit, { healthy: 'a bit' });

      stubbedRedFruit.healthy;
      // 'a bit'
      stubbedRedFruit.color;
      // 'red'
      stubbedRedFruit.cutInPieces();
      // 2
      redFruit.healthy;
      // 'yes'
      // You get the point
    {% endhighlight %}

### Mocking

    {% highlight javascript %}
      var mockFruit = smoking(redFruit).expects({ cutInPieces: 1 });
      mockFruit.cutInPieces();
      smoking(mockFruit).verify();
    {% endhighlight %}

## VCR.js
[https://github.com/elcuervo/vcr.js](https://github.com/elcuervo/vcr.js)

![VCR](/images/posts/rats/vcr.jpg)

It inherits most of the method names and the functionality from ruby
[VCR](https://github.com/myronmarston/vcr)
which it's an amazing tool, you should use it.

Sometimes you want to test objects that make use of ajax requests and sometimes
you don't want to have the backend server or just add stupid data when trying to
try other things.

I find it very usefull when trying to test a
[Backbone](http://documentcloud.github.com/backbone/) Model which, no matter
which of the side libraries you use (jQuery, Ender, Zepto, etc) you finally use
a XMLHTTPRequest object, so VCR.js intercepts the response and saves it so next
time you run the test you'll have *real* data.

    {% highlight javascript %}
      scenario("Ajax interception", {
        'setup': function() {
          VCR.configure(function(c) {
            c.hookInto = window.XMLHttpRequest;
          });
        },

        'Recording ajax request': function(g) {
          VCR.useCassette('test', function(v) {
            XMLHttpRequest = v.XMLHttpRequest;

            var makeRequest = function() {
              var ajax = new XMLHttpRequest();

              ajax.open('GET', 'test.html');
              ajax.onreadystatechange = function() {
                if(ajax.readyState === 4) {
                  g.assertEqual("Hello World!\n", ajax.responseText);
                }
              };
              ajax.send(null);
            }
            // Record First Request
            makeRequest();

            // Wait for it...
            g.setTimeout(function() { makeRequest(); }, 100);
          });
        }
      });
    {% endhighlight %}

Eventually I plan to add suport to WebSockets so network related test can be run
without any problems.

I plan to write a blog post showing real-life scenarios using Gerbil, Smoking
and VCR.js
