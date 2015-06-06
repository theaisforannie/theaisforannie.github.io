---
layout: post
title: inject for beginners (part 1 of 2)
---

Unfortunately, as you probably already know, Ruby. To be absolutely clear, I really like it (though I have little to compare it against), but it has its ... idiosyncracies. For example, there are approximately 837 methods that will return you the size of a list:

	array = [1, 2, 3, 4]

	array.count
	=> 4

	array.size
	=> 4

	array.length
	=> 4

But should you want to add all the numbers in `array` together, you will find exactly zero methods in Ruby called `sum`.

Fortunately [(?)](https://media0.giphy.com/media/3vc4sSq0l8AbS/200.gif) there's a completely opaque and advanced Ruby solution that you can use to perform literally the simplest mathematical operation on a list of numbers in the most convoluted way possible. It's called [`inject`](http://ruby-doc.org/core-2.2.2/Enumerable.html#method-i-inject).

With thanks to my friend [nelhage](https://twitter.com/nelhage) (and apologies where they are due -- any inaccuracies are most definitely mine), let's do some simple addition:

	array = [1, 2, 3, 4]
	array.inject(0) { |sum, x| sum + x }
	=> 10

Don't panic. We're gonna get through this together.

`inject` takes both an argument and a block. (The argument I've specified, `0`, is actually the default, meaning I could have left it out entirely and Ruby would have been just fine with that. I included it for illustrative purposes, but the cool thing is if I had made it `1`, the whole thing would have returned 11. Play around in pry and see for yourself.) 

The block is just a bit of code like others you've seen. It takes two arguments of its own, which I've named `sum` and `x`, but these are of course arbitrary and while I was playing around with `inject` I literally named them `grapes` and `bananas` for a bit just to prove their arbitrariness to myself. (But remember, computer programs are handcranked _by people_, so give your variables and arguments names that are meaningful _to people!_) `sum` is the result of what's happening in the block, so the first time it runs (before the block has had a chance to do anything), `inject`'s argument (in this case `0`) is the value of `sum`. `x` is the next array item in sequence.

^^^^ but that's all super confusing. Let's walk through what's _actually_ happening. Recall:

	array = [1, 2, 3, 4]
	array.inject(0) { |sum, x| sum + x }

+ `inject` calls the starting value `0` and conveniently sticks that in `sum`. It adds `sum` to the first value in our array, `1`, meaning where `sum = 0` and `x = 1`, `sum + x = 0 + 1`. This returns a _sum_ of `1`, which is looped back into the block now as the new value of `sum`.

+ Now in our second pass through the block, `sum = 1` and `x = 2`, the next value in the array. So now `sum + x = 1 + 2`. This returns a value of `3`, which is passed back into the block as the _new_ new value of `sum`.

+ On our third iteration, `sum` is `3` and now `x` is `3` (the third item in our array). So `sum + x = 3 + 3` returns of value of `6`, which is passed back into the block as the next value of `sum`.

+ Now `sum` equals `6` and the value of `x` is `4`, the fourth (and final) value in the array. Therefore, `sum + x = 6 + 4`, which returns a final value of `10`.

`=> 10`

Congratulations! You are now equipped to define your _own_ `sum` method:

{% highlight ruby %}
def sum(array)
  array.inject(0) { |sum, x| sum + x }
end
{% endhighlight %}

Turns out there's more cool stuff you can do with `inject`! But I'm saving it for my next post because there's already enough words on this page.