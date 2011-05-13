---
layout: post
title: Cuba Libre chico
published: true
categories: [web-framework, cuba, sugar, ruby]
image: "/images/posts/cuba-libre-chico/image.jpg"
featured-image: "/images/posts/cuba-libre-chico/featured-image.jpg"
summary: I've been wandering in the Ruby world a year now, from Rails to Sinatra. But sometimes i'd really want things simpler and perhaps more clean of developing web applications. Specially when you can get something fast and get total control. Thats when i've found Cuba a great web framework.

---

{{ page.summary }}

In that spirit [Cuba](https://github.com/soveran/cuba) its a powerful web framework aiming simplicity, speed and clean syntaxt.
Cuba inherits all its power from [Rack](https://github.com/chneukirchen/rack) and [Rum](https://github.com/chneukirchen/rum) but now overpowers it to make it even cooler.
Being refactored some time ago now looks a lot like [Sinatra](https://github.com/sinatra/sinatra) routes, here's an example

{% highlight ruby %}
require 'cuba'

Cuba.define do
  # /
  on "" do
    res.write "Home page"
  end

  # /users/:username
  on "users/:username" do |username|
    user = User.find_by_username(username)

    # GET /users/:username
    on get do
      res.write "Hello, i'm #{user.name}"
    end

    # POST /users/:username w/params[:user]
    on post, param('user') do |query|
      if user
        user.update_attributes query
        res.write "user: #{user.id} updated!"
      end
    end

  end

end

{% endhighlight %}

This encourages a cleaner and simpler way of writing routes, besides it also includes the power of [Tilt](https://github.com/rtomayko/tilt.git)

{% highlight ruby %}
require 'cuba'

Cuba.define do
  on "register" do
    @message = "Hello dude!"
    res.write render("templates/register.erb")
  end
end
{% endhighlight %}

I liked Cuba so much i started a little lib to include all the contrib of Cuba, and i called it [cuba-sugar](https://github.com/elcuervo/cuba-sugar)

![Cuba Sugar](/images/posts/cuba-libre-chico/cuba-sugar.jpeg)

Which adds some things not so much, just to make writing a little easier following the way Cuba is doing stuff right now.

{% highlight ruby %}
require 'cuba'
require 'cuba/sugar'

Cuba.define do
  on "faq" do
    # You can output the return of the block as the response
    as do
      "This are the most common questions"
    end
  end

  on "fancy" do
    @captcha = 123456
    as { render('templates/captcha.haml') }
  end

  on "coffee" do
    # And you can choose what HTTP code and extra headers
    as 419, {'Content-Type' => "text/teapot"} do
      "I DONT KNOW :(((("
    end
  end

  on "users/:username/json" do |username|
    user = User.find_by_username(username)
    as_json do
      {
        name:           user.name,
        location:       user.location,
        is_it_friday:   Time.now.friday?
      }
    end
  end

end

{% endhighlight %}
