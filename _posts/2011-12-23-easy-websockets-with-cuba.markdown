---
layout: post
title: Easy websockets with Cuba
published: true
categories: [cuba, websockets]
hashtags: cuba, websockets

---
![Websockets](/posts_assets/websockets-header.png)

I have the urge to find simple solutions to complex problems, in that mad quest i've found myself reading extremely long libraries that do things in complicated ways just to have more 'features'. Nowdays the web only wants to add realtime in any sentence and thanks to HTML5 and Websockets it's a pretty easy thing to do but... doing that with ruby gets overcomplicated.

Trying to find a simple solution i've researched a lot. I've found
[Cramp](http://github.com/lifo/cramp) to work pretty well, but requiring
activesupport it's against my religion :D.

After some googling and githubbing i've found
[websocket-rack](https://github.com/imanel/websocket-rack) which is a clean and
simple solution for a problem that it's actually simple.


{% highlight ruby linenos %}
require 'cuba'
require 'rack/websocket'

class Websockets < Rack::WebSocket::Application
  def on_open(env)
    send_data("Welcome")
  end

  def on_message(env, message)
    send_data(message.reverse)
  end
end

Cuba.define do
  on "ws" { run Websockets.new }

  on "" do
    res.write "The websockets are in /ws"
  end
end
{% endhighlight %}

This is a silly example, but enough in most of the cases, in here you will have
to choose a way to id the clients and send them some messages but that's the fun
part.
