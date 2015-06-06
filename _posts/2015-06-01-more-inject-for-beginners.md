---
layout: post
title: more inject for beginners (part 2 of 2)
---

Now that we know [how to use `inject` to sum an array]({% post_url 2015-05-31-inject-for-beginners %}) of numbers in Ruby, let's see what other crazy things this method can do by building a weird hash! The _actual_ results will be useless, but the breakdown will hopefully be illustrative.

Let's gather our supplies. In order to do this, we're going to need an array of stuff and an empty hash. We can create these pretty easily:

{% highlight ruby %}
# our array
beatles = ["john", "paul", "george", "ringo"]

# our empty hash
beatles_hash = {}
{% endhighlight %}

Now the cool part. (I hope you're following along in pry.)

{% highlight ruby %}
beatles.inject(beatles_hash) do |x, y|
  x[y] = y.reverse
  x
end
{% endhighlight %}

Then when we call `beatles_hash` it returns 

`=> {"john"=>"nhoj", "paul"=>"luap", "george"=>"egroeg", "ringo"=>"ognir"}`

whaaaaat.

`inject` is pretty neat, you guys. Let's talk about what happened.

First of all, recall that when you have a hash, you can add a key-value pair to it just by typing `existing_hash[new_key] = new_value`. Concrete example:

{% highlight ruby %}
capitals = {
  "California" => "Sacramento",
  "Wisconsin" => "Madison",
  "Indiana" => "Indianapolis"
}

# adding a new key-value pair
capitals["Missouri"] = "Jefferson City"

# our hash now looks like so
=> {"California"=>"Sacramento",
    "Wisconsin"=>"Madison",
    "Indiana"=>"Indianapolis",
    "Missouri"=>"Jefferson City"}
{% endhighlight %}

And if I have an _empty_ hash, like the one we started with in the Beatles example, I can do the same:

{% highlight ruby %}
new_hash = {}
=> {} # see? it's empty

# let's fix its emptiness
new_hash["foo"] = "butts"

new_hash
=> {"foo"=>"butts"} # now it's not! \o/
{% endhighlight %}

Back to inject. Here's the code again:

{% highlight ruby %}
beatles.inject(beatles_hash) do |x, y|
  x[y] = y.reverse
  x
end
{% endhighlight %}

Just like in my [previous]({% post_url 2015-05-31-inject-for-beginners %}) post about summing, the first argument in the block takes the value of the argument to inject, in this case _an empty hash_. Its first iteration looks like this:

{% highlight ruby %}

# adding the john key-value pair
{}["john"] = "john".reverse

# the hash itself is no longer empty,
# as in our {"foo" => "butts"} example above
# x is now a hash with one key-value pair

# we then return x so that it goes back up into the loop
# looking like such:
{"john"=>"nhoj"}
{% endhighlight %}

Time for a second pass. Having returned x with one key-value pair, we now add paul:

{% highlight ruby %}
{"john"=>"nhoj"}["paul"] = "paul".reverse

# returning the hash back into the loop
{"john"=>"nhoj", "paul"=>"luap"}
{% endhighlight %}

Then we do the same with george and ringo in turn:

{% highlight ruby %}
{"john"=>"nhoj", "paul"=>"luap"}["george"] = "george".reverse
{"john"=>"nhoj", "paul"=>"luap", "george"=>"egroeg"}

# this is getting cumbersome
# if only i had a built-in ruby method to do it for me
# i could blog about it
# i would be so popular at parties
{"john"=>"nhoj", "paul"=>"luap", "george"=>"egroeg"}["ringo"] = "ringo".reverse
{"john"=>"nhoj", "paul"=>"luap", "george"=>"egroeg", "ringo"=>"ognir"}
{% endhighlight %}

Et voila! The example is contrived -- I don't know why you'd want to create a hash that looks like this -- but that's not the point. The point is that I find `inject` to be unusually opaque and I am trying to make it marginally clearer.

But you know, if you're looking for ideas you could write your own method and do that. Or you could use Ruby's built-in `hash` function (not to be confused, of course, with Ruby's hash _object_, which we've been discussing) to turn your grocery list into secrets only you know:

{% highlight ruby %}
groceries = ["tofu", "spinach", "apples", "pretzels", "bourbon"]
secrets = {}

groceries.inject(secrets) do |x, y|
  x[y] = y.hash
  x
end

secrets = {
  "tofu"=>-1708800852484854388,
  "spinach"=>3861562576586590805,
  "apples"=>2254505784032821577,
  "pretzels"=>1056526202726194867,
  "bourbon"=>-757686621161009524
}
{% endhighlight %}

Try *that* one at your next cocktail party.

(BTW, more thanks are owed to my pal [nelhage](https://twitter.com/nelhage) for enthusiastically walking me through this in the first place. As before, any inaccuracies are one million percent mine, unless he was wrong in the first place, in which case _nelhaaaaaaage_. No but seriously: <3 thanks! <3)
