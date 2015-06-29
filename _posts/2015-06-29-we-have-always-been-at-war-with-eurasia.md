---
layout: post
title: '"We have always been at war with Eurasia": Class Variables in Ruby'
---

Class variables! What are they, even.

Class variables are variables whose value changes across instances! You can recognize them because they are prepended with `@@`.

But that definition is pretty opaque. Let's use an example of a time when information gets uniformly (and rapidly) changed!

With apologies to anyone who's read *[1984](https://en.wikipedia.org/wiki/Nineteen_Eighty-Four)* more recently than -- aw geez -- almost 20 years ago, here's a spoiler-less summary of what you need to know to follow along:

In *1984* there's this concept called Doublethink, wherein the people manage to basically avoid contradictory thought. The government fights a perpetual war alongside or against one of two other states -- Eurasia and Eastasia. The history books are re-written every time an alliance changes, hence the doublethink-inspired idiom, "We have always been at war with Eastasia." 

We can imagine this rewriting of history with Ruby's class variables because **changing the value of a class variable changes across all instances of that class**. You see:

{% highlight ruby %}
class Doublethink
  @@always_at_war_with = "Eurasia"

  # returns the value of @@always_at_war_with
  # when we begin, it's Eurasia
  def always_at_war_with()
    @@always_at_war_with
  end

  # sets the value of @@always_at_war_with
  # we can change this arbitrarily,
  # as you will see
  def enemy(them)
    @@always_at_war_with = them
  end
end
{% endhighlight %}



{% highlight ruby %}
# let's write a history book
history_book = Doublethink.new
history_book.always_at_war_with # => "Eurasia"

# what do the newspapers say?
newspaper = Doublethink.new
newspaper.always_at_war_with # => "Eurasia"

# oh but wait
newspaper.enemy("Eastasia")

# now who are we always at war with?
# let's ask the newspaper
newspaper.always_at_war_with # => "Eastasia"

# and what does our history book tell us?
history_book.always_at_war_with # => "Eastasia"

# what about a new history book we just wrote?
new_history_book = Doublethink.new
new_history_book.always_at_war_with # => "Eastasia"

# and if our new history book determines
# we've always been at war with Eurasia, guess what?
new_history_book.enemy("Eurasia")
new_history_book.always_at_war_with # => "Eurasia"
history_book.always_at_war_with # => "Eurasia"
newspaper.always_at_war_with # => "Eurasia"

{% endhighlight %}

We can go back and forth like this indefinitely. (It's a perpetual war, after all.) There isn't a single instance of the class Doublethink that won't have a uniform value for the class variable `@@always_at_war_with`. Cool, right?

You can also do some Doublethink with instance variables, but that's for another day:

{% highlight ruby %}
class Doublethink
  attr_reader :twoplustwo

  def initialize()
    @twoplustwo = 5
  end
end
{% endhighlight %}

*Thanks to [Richo](https://twitter.com/rich0H) for helping me understand class variables (finally)*


