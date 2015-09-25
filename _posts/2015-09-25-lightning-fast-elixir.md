---
layout: post
title:  Lightning Fast Elixir
date:   2015-09-25 12:00:00
categories: posts
---

I have this arbitrary career goal: I want to make a back end that serves millions of requests and does it blazingly fast. Having been a Rails developer for most of my programming career, I've been branching out to learn a new language. As you can see from my past posts, I was dabbling with [Golang](https://golang.org/). I even wrote [http://gruuub.com](http://gruuub.com) with it. It's fast for sure, but I wanted to see what else was out there.

Enter [Elixir](http://elixir-lang.org/) and [Phoenix](http://phoenixframework.org). Elixir is a new language built off of Erlang. Phoenix is a web framework for Elixir. And it's fast. Lighting fast. Serve pages in *micro*seconds fast.

## First Impressions
Functional programming is weird. I tried to do an [exercism.io](http://exercism.io/) in Elixir and utterly failed. After that, I decided that some reading was in order. I bought [Programming Elixir](https://pragprog.com/book/elixir/programming-elixir) and started reading it, and that helped a ton. I could actually write some Elixir code after that. Nice. On to making a web app!

Phoenix is an MVC framework heavily inspired by Rails. In fact, I found that a lot of features of Rails are already present in Phoenix. Migrations, the awesome router we all know and love (albeit slightly different), even *the flash*. It's a full-featured web framework for sure. Getting it up and running equally simple. Install Elixir, install Phoenix, generate an app, create your database, and run `mix phoenix.server`. Pretty simple.

## A Test Drive
After I commited to a hack-a-thon that [QuickLeft](http://quickleft.com) (my employer) was hosting, I decided to use Phoenix for the back end of my team's web app. Why not? I had yet to use it for something real and I didn't really care about winning. So I used it. The result? I was surprisingly productive. We only had about 2 - 3 hours to write something. In that time, I spun up a web server with a couple JSON API endpoints that served (mocked) data for the JavaScript front-end.

I tried utilizing the database, but failed. I created 3 models with migrations, but ultimately failed to implement the controller actions for them correctly, so I scrapped it and just used mock data. But holy Batman, making those models and migrations was dead-simple because of the generators.

## Speed
If I were 1) using the database and 2) serving from a real server (not localhost), I'm guessing this would be slower. But I don't think it would slow down *that* much. Here's the server log with response times:

{% highlight bash %}
[info] GET /api/shops
...
[info] Sent 200 in 756µs
[info] GET /api/shops
...
[info] Sent 200 in 774µs
[info] GET /api/shops
...
[info] Sent 200 in 752µs
[info] GET /api/shops/1
...
[info] Sent 200 in 920µs
{% endhighlight %}

Behold, _MICROSECONDS_. Have you ever seen that `µ` symbol in a server log before? My team mate had to Google it. Yeah...lightning fast.

## TL;DR
Phoenix is lighting fast but is still just as productive as Rails. More to come when I get something working in production. You should also hire [QuickLeft](http://quickleft.com) to make something with Elixir so I can get paid to work with it more.
