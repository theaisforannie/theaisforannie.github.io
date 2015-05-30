---
layout: post
title: Combining arrays of tuples
---

Let's say you've got an array of tuples like this:

`array_of_tuples = [[1, 2], [3, 4], [5, 6]]`

and another array of tuples like this:

`another_array_of_tuples = [[7, 8], [9, 10]]`

What you _want_ is for all five tuples to live happily in one array together, but when you do this:

`big_array = array_of_tuples.push(another_array_of_tuples)`

_Uggggggghhhhhhhhhhh_ this happens:

`[[1, 2], [3, 4], [5, 6], [[7, 8], [9, 10]]]`

`push` has combined the two arrays, sure, but it's also left the second array in tact in the form of those extra brackets around 7, 8 and 9, 10.

Oh but you. You're clever. You know how to `flatten`.

`big_array.flatten`

Whoops:

`[1, 2, 3, 4, 5, 6, 7, 8, 9, 10]`

No more tuples. Just an array. (Good thing you didn't `flatten!`.)

Oh! Ohhhhh! But you can add [an optional argument](http://ruby-doc.org/core-2.2.0/Array.html#method-i-flatten) to `flatten` to "determine the level of recursion". You're not _exactly_ sure what this means, but you push up your sleeves and get to work anyway.

`big_array.flatten(1)` doesn't do what we wanted though. It returns `[1, 2, 3, 4, 5, 6, [7, 8], [9, 10]]`. Whomp.

You just start typing numbers in:

`big_array.flatten(2)` does the same as `flatten`. So does `big_array.flatten(-1)`. (You also try 3 for good measure. Same result. You wonder whether or not to admit to this later.)

At this point you push the computer away from your hands and lay your face on the table. How much time have you spent on this? _All you want_ is to add two arrays of tuples into one thing.

Oh wait.

Hang on a minute.

`array_of_tuples + another_array_of_tuples`

`[[1, 2], [3, 4], [5, 6], [7, 8], [9, 10]]`

You laugh like an insane person at the simplicity of this, but look at all you learned! Enjoy your victory sip.

