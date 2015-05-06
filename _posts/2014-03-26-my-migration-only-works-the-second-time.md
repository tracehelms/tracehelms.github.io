---
layout: post
title:  My Migration Only Works The Second Time
date:   2014-03-26 10:44:00
categories: posts
---

You want to write a Rails 3.2 migration that also transforms some data, you say? You might want to try something like this:

{% highlight ruby %}
def up
  add_column :fruits, :is_banana, :boolean
  Fruit.where(name: "banana").each do |f|
    f.update_attributes(is_banana: true)
  end
end
{% endhighlight %}

And you would think this is correct. It looks very correct, indeed. But let's suppose you run a migration AFTER this one that uses the is_banana column you added. Perhaps something like this:

{% highlight ruby %}
def up
  Fruit.create(name: "Apple", is_banana: false)
end
{% endhighlight %}

If you run these two migrations at the same time, the second one will fail saying `unknown attribute is_banana`. If you run it a second time, it will succeed.

## Why?

Rails keeps a cache of which tables have which columns, something that gets updated when you add a column in a migration. Furthermore, it seems like having the line

{% highlight ruby %}
Fruit.where(...)
{% endhighlight %}

prevents Rails from immediately updating the cache of columns on that table.

So when we go to update that model in the next migration, Rails thinks the `is_banana` column doesn't exist, giving you an `unknown attribute is_banana` error. When you run the migration a second time, Rails gets a chance to update the cache and it succeeds.

To fix it, we can use the method [`reset_column_information`](http://apidock.com/rails/ActiveRecord/ModelSchema/ClassMethods/reset_column_information).
Let's see it in use:

{% highlight ruby %}
def up
  add_column :fruits, :is_banana, :boolean
  Fruit.reset_column_information
  Fruit.where(name: "banana").each do |f|
    f.update_attributes(is_banana: true)
  end
end
{% endhighlight %}

Tada! Your migrations should now work when running them together.
