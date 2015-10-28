---
layout: post
title:  Pattern Matching In Elixir
date:   2015-10-27 12:00:00
categories: posts
---

_This post can be found on the [Quick Left blog](https://quickleft.com/blog/pattern-matching-elixir/)._

## Introduction
Learning a new language is a great way to expand your knowledge about programming. You'll usually pick up some cool tricks or a new way of thinking about things. One of the things I've learned that's different about Elixir is [pattern matching](http://elixir-lang.org/getting-started/pattern-matching.html). It comes from its Erlang roots and it seems like one of those things that I'll wish every other language had from now on.

## What Is Pattern Matching?
In Elixir, the `=` sign doesn't necessarily mean "set this variable equal to something else". Instead, it means "match the left hand side to the right hand side". Usually you can declare things like `x = 2` and never notice a difference. At some point though, you'll start to notice something is quite a bit different.

```elixir
iex> x = 2
2
iex> y = 3
3
iex> 2 = x
2 # waaaat?
iex> 2 = y
** (MatchError) no match of right hand side value: 3
```

No, `2` is not being set equal to `x`. It's being pattern matched against it! The difference is subtle now, but let's go on to see how useful it can be.

### A Note About Immutable Variables
Elixir is a functional language which means that variables can't change value. You can, however, reuse variable names. If you set `x = 2` and then set `x = 3`, Elixir doesn't mind.

```elixir
iex> x = 2
2
iex> y = 3
3
iex> x = y
3
iex> x
3
iex> ^x = 2 # force no reusing of variables
** (MatchError) no match of right hand side value: 2
```

Behind the scenes, though, there's still a version of `x` that is set to `2`. For instance, if you pass `x` to another function that's being run concurrently and then change `x`, the concurrent function will still have the original value of `x`. You can force variables to not be reused by using `^`, but `^x = 3` would still match because `x` actually is `3`.

## Basic Pattern Matching
Let's say you have a tuple and want to assign its different values to some variables.

```elixir
iex> {result, value} = {:ok, 2}
{:ok, 2}
iex> result
:ok
iex> value
2
```

Cool. We can pick up the different values and assign them to variables. But what if we only want to assign `value` if the result is `:ok`?

```elixir
iex> {:ok, value} = {:ok, 2}
{:ok, 2} # this passed the pattern match
iex> value
2
iex> {:ok, value} = {:nope, 2}
** (MatchError) no match of right hand side value: {:nope, 2}
```

This actually matches the `:ok` from the left hand side to the right hand side. `value` only gets assigned because the entire tuple (including the `:ok`) matches. The second one doesn't match because `:ok` is not `:nope`. Again, this might seem pretty useless, but look at how we can use this in real code:

```elixir
# my_case.exs
defmodule MyCase do

  def do_something(tuple) do
    case tuple do
      {:ok, value} ->
        "The status was :ok!"
      {:nope, value} ->
        "Nope nope nope nope..."
      _ ->
        "You passed in something else."
    end
  end

end
```

Then load up the file in `iex` by running `$ iex my_case.exs`.

```elixir
iex> MyCase.do_something({:ok, true})
"The status was :ok!"
iex> MyCase.do_something({:nope, true})
"Nope nope nope nope..."
iex> MyCase.do_something({:wat, true})
"You passed in something else."
```

I hope it's becomming apparent that pattern matching is a useful tool in Elixir. If you use it right this can help make your code easier to read.

## Pattern Matching In Function Definitions
This is where pattern matching truly shines and you'll use it all the time in Elixir. You can declare functions that will only be executed if the arguments match the patterns. Let's take a look.

```elixir
# talker.exs
defmodule Talker

  def say_hello(:bob), do: "Hello, Bob!"
  def say_hello(:jane), do: "Hi there, Jane!"
  def say_hello(name) do # name is a variable that gets assigned
    "Whatever, #{name}."
  end

end
```

Then load up the file in `iex` by running `$ iex talker.exs`.

```elixir
iex> Talker.say_hello(:jane)
"Hi there, Jane!"
iex> Talker.say_hello(:bob)
"Hello, Bob!"
iex> Talker.say_hello("Trace")
"Whatever, Trace."
```

As you can see, three different methods are actually called based on what's passed into them! This is laying the foundation for doing some really awesome things. Recursion in Elixir, for example, relies heavily on pattern matching. Let's take a look.


## Uses In Recursion
Let's define a function that will recursively square every number in a list. We start by defining the base case which is when we pass in an empty list `[]`. Then we define the function that actually does the squaring.

```elixir
# my_squarer.exs
defmodule MySquarer do

  def square([]), do: []
  def square([head | tail]) do
    [head * head | square(tail) ]
  end

end
```

In Elixir, you can express a list as the first component (the head) and then the rest of the list. The last element is always an empty list. So when we say `[head | tail]`, we are quite literally matching the first item in the list to `head` and the rest of the list to `tail`.

```elixir
# example showing what head and tail are
iex> [head | tail] = [1, 2, 3, 4]
[1, 2, 3, 4]
iex> head
1
iex> tail
[2, 3, 4]
iex> [head | tail] = [1]
[1]
iex> head
1
iex> tail
[]
```

So back to our `square/1` function. It returns a new list with the squared `head` as its first element and recursively calls `square/1` to fill in the rest of the list. After the last element is reached, the function definintion `square([])` will match an empty list and return an empty list. That's when the function call chain will complete and return the result.

Let's see this in action. Load up the file in `iex` by running `$ iex my_squarer.exs`.

```elixir
iex> MySquarer.square([])
[]
iex> MySquarer.square([1, 2, 3])
[1, 4, 9]
iex> MySquarer.square([-1, -2, -3])
[1, 4, 9]
```

## Wrap Up
Hopefully you now have a clearer understanding of what pattern matching in Elixir means and how it can be useful. Recursion is tricky but Elixir's pattern matching helps us to make it a little easier to read and understand.

If you have any questions, leave them in the comments!
