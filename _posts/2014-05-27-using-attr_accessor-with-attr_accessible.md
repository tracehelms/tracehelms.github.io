---
layout: post
title:  Using attr_accessor with attr_accessible in Rails
date:   2014-05-27 10:44:00
categories: posts
---

I've found using `attr_accessor` on Rails projects is a stumbling block for many developers. We've been using `attr_accessible` our entire Rails career, but then we jump on a project and see `attr_accessor` defined in a Rails model. What the ...? In this post we'll talk about how to use `attr_accessor` with Rails' `attr_accessible`, where it can be useful, and things to watch out for.

## attr_accessor

Plain and simple, `attr_accessor` is a plain old Ruby way to define getter and setter methods. It's essentially `attr_reader` and `attr_writer` combined. Specifically:

{% highlight ruby %}
class Post
  attr_accessor :content
end

# Is equivalent to...

class Post
  @content

  def content=(value)
    @content = value
  end

  def content
    @content
  end
end
{% endhighlight %}

Again, `attr_reader` only defines the getter, the `content` method in this case, and `attr_writer` only defines the setter, the `content=` method. `attr_accessor` defines them both. And an instance variable. Woot!

This is great for setting instance variables on objects in Rails that you don't plan on persisting.

## attr_accessible

_(Warning, this applies to Rails 3.2. I'm pretty sure this changes because of strong parameters in Rails 4+, but I'm not exactly sure how. Updates to come.)_

We all know and love `attr_accessible`, but what does it do? In Rails 3.2, it's very similar to `attr_accessor` as it defines getters and setters, but it also lets you mass assign attributes. What does that mean? It simply means you can update multiple attributes at the same time, like you would with a params hash from within a controller. An example:

{% highlight ruby %}
# This is usually passed in from a view
params = blog: {content: "Hello World!", name: "First Post"}

def create
  @blog = Blog.new(params[:blog])
  if @blog.save
  # Rest of the method omitted...
end
{% endhighlight %}

When you specify `attr_accessible`, Rails assumes that this attribute has a corresponding column in your database, and defaults to saving it there. This means that your Post class tries to save the content attribute in `posts.content` in your database. Makes sense.

## Combining The Two

When you define `attr_accessor` __with__ `attr_accessible` on the same attribute, you are telling Rails a couple things. First, you want to be able to mass assign this attribute. Second, that you don't want to persist this attribute in the database, but store it in an instance variable instead. This can be useful for dictating logic in your model, like so:

{% highlight ruby %}
class Post < ActiveRecord::Base
  attr_accessible :content, :name, :publish_immediately
  attr_accessor :publish_immediately

  after_create :publish!, if: :publish_immediately

  def publish!
    # omitted...
  end
end
{% endhighlight %}

In this example, the Post class will immediately publish this post upon creation. The attribute `publish_immediately` would perhaps represent a checkbox on the create post form. Assuming we don't want to persist this, the previous code block would let us store the value of the checkbox in an instance variable, which we access on the `after_create` line. Again, `attr_accessor` _intercepts_ Rails' attempt to store the attribute in the database, and stores it in an instance variable instead.

## Closing Thoughts

There are probably situations where this would come in handy, but my personal recommendation is to go ahead and persist your attributes. Database storage is cheap and you never know when your boss (or customer) will come to you and say, "Hey, was that one post published immediately?".
