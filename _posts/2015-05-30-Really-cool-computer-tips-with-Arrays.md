---
layout: post
title: Really Cool Computer Tips with Arrays
---


A Cool Thing you can do when you are programming computers is to make a list of things!

	**Really Cool Computer Tips**

	In computers, we call lists "arrays" because we like to make our brains work harder unnecessarily!

	When a computer person is like "Blah blah I put them in an array" all that person means is "I took the things that we're talking about and made a list!"

Here are some lists I made:

	`arbitrary_numbers = [1, 2, 3, 4]`

	`grocery_list = ["bananas", "spinach", "tofu", "bourbon"]`

Lists on their own are kind of boring! Let's do cool stuff to our lists!

	**sum the numbers in a list**
	`arbitary_numbers.inject(:+)`
	`=> 10`

Ok so check this out: Ruby's got like 18 different ways to count the items in a list (count, length, size), but exactly zero methods called `sum`. You can use the inject method to get around this super weird oversight!

You can also build a hashmap _from your array_! For kicks![0]

	**SOOOPER SEKRIT GROCERY LIST**
	sekrits = {}
	grocery_list.inject({}) do |x, item|
		x[item] = item.hash
		x
	end

To recap: Array is just a fancy word for list, _and_ you can *inject* them with things which always sounds to me like you're putting motor oil in!


0: Or for actual practical reasons! Up to you!

