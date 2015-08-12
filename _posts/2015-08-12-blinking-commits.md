---
layout: post
title: Blinking Commits
---

Last time on ugh blog dot jpg etc, several many thousands of internet people ([in, like, a day](http://hnrankings.info/9894508/)) came here to read about [old protocols]({% post_url 2015-07-15-three-dead-protocols %}). That was very exciting, and I was gobsmacked by all the nice things everyone said! Unfortunately, it had the weird side effect of making it nearly impossible to write anything new ... until today. (omg are you excited?)

An aside before we begin: There is literally no reason to ever do this at all ever. It is a stupid idea. But if you're into that, read on.

<h3>
  <div class="blink">How to Make a Blinking Commit Message</div>
</h3>

All you need is a git repo and a change to commit. (For the purposes of this "tutorial," I am assuming you write your commit messages in vim. Like, idk if it works in emacs, you can lmk, w/e.)

Kk, so get all your git things staged because the party starts here:

`git commit`, hit enter, congratulations, you have now entered vim! Go into `insert` mode and hit `Ctrl + v`, followed by the `esc` key. (Don't be like me and try `Ctrl + v + esc` all at once. That isn't a thing and won't work.) This tiny keystroke combo is how you escape `esc`! 🆒

`^[` should have appeared on the screen! (If it didn't, make sure you're in insert mode and try again!) Pretty neat, huh? Now type

`[5myour commit message here`, do `Ctrl + v`, `esc` again, then type `[0m`

Your whole message will look like this:

`^[[5m#yolo^[[0m`

Now save the commit, et voilà! `git log` and you'll see:

<p><div class="blink">
  <code>#yolo</code>
</div></p>

And that, my party compatriots, is how to end six weeks of Hacker News-induced writer's block. As always, thank you for reading.

_With thanks to [the incomparable Tom Ballinger](https://twitter.com/ballingt) for showing me ANSI escape sequences and [Allie Jones](https://twitter.com/gredaline) and [Gonçalo Morais](https://twitter.com/gnclmorais) for help making this page blink. (All complaints should be addressed to me.)_
