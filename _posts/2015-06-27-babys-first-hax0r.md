---
layout: post
title: Baby's First Hax0r
---

The engineers who taught me to program were by and large members of the Stripe's security and sys[^0] teams. Important lessons I was taught:

* Don't publish your secret keys.
* Don't type commands into your terminal you don't understand or haven't verified as coming from a trusted source.
* Know how to verify a trusted source.
* Don't plug random USBs into your computer. ([ಠ_ಠ](http://www.twitter.com/tetrakazi)) (jkjkjkjkjk, love you karla!)
* Use 1Password, ffs.
* Lock your computer.

That last one's really important, and anyone can do it, programmers and non-programmers alike.

Trouble is, I see unlocked, unsupervised laptops *everywhere*. I giggle at the thought of what I could do.

Now, I'm not about to hack a stranger's computer to make a point. I'm not a monster. That is why I waited until I'd made a friend at RC to do it.

Everyone, meet Steven.
======================

![Hi Steven!]({{ site.url }}/assets/steven.jpg)

Let me just say that Steven is a really great, super smart person with as-yet infinite reserves of patience for me (as we shall see). Even after doing this, he still promises to teach me to waltz. You may remember his previous appearance on our show in our episode [(marginally) about databases]({% post_url 2015-06-18-12000-butts %}). 

To further demonstrate that I Am Not A Monster, I asked him for his permission to out him and his #yolo attitude toward computer security while I was writing this. His response:

> You can totally out me as a happily trusting individual that believes in the best of humanity and totally isn't a locky cynic :P

Steven leaves his laptop unlocked all the time and it takes like a day and a half for his computer to go to sleep.

And for the record, I share Steven's view of the goodness of people. But I also know, at least conceptually, [what a rainbow table is](https://twitter.com/anyharder/status/564156968458133504); I've seen how emails are forged; and yes, I've had mystery USB sticks handed to me (you're the best, karla! luv u most! xoxoxoxo).

And I know what dedicated security engineers look like when they're under attack, and I've seen some of the steps they take to prevent those scenarios. I've watched them combat actual monsters. (It ... involves a lot of serious faces, furious typing, and cuddling amigurumi. Programming is not an action movie.)

Wednesday Steven was sitting next to me and got up to go wherever. I looked around. He was nowhere to be seen. I rolled my chair in front of his computer and opened Spotlight so that I could quickly get to his terminal app.

Strange characters appeared on the screen. Dvorak! I wasn't expecting this and am unfamiliar with the keyboard layout. I was thwarted, but I decided he should know what I had tried to do.

Friends, readers: when he came back to the table, I told him *explicitly* my intention was to make mischief on his computer. We talked about it. We talked about [hot corners](http://it.emory.edu/security/screensaver_password.html) and I showed him how I use [Alfred](http://www.alfredapp.com) to lock my computer quickly. I told him I would make another attempt.

*I told him, you guys.*

And that is why, a few hours later, I was able to post this to twitter:

<blockquote class="twitter-tweet" lang="en"><p lang="en" dir="ltr">Achievement unlocked: first hax0r 🏆🎉🎈👑</p>&mdash; ann(ie)?$ (@anyharder) <a href="https://twitter.com/anyharder/status/613844744901410816">June 24, 2015</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

Here's how I did it
===================

Steven left with a group of people to grab dinner. I was busy learning about Ruby classes from another RCer. I don't even know how long he was gone before I noticed.

This time I was prepared. Because I couldn't type on his keyboard I navigated to System Preferences and opened Keyboard.

![System Preferences pane]({{ site.url }}/assets/system_pref.png)

From there I clicked on the Input Sources tab and added the Canadian English keyboard because it was the first one I saw that I knew I'd be able to type on.

![adding a Canadian English keyboard]({{ site.url }}/assets/keyboard.png)

*With apologies to my Australian and British friends. I had to move quickly.*

First hurdle jumped, I went to his terminal and hoped he was a Homebrew user.

`$ brew install sl`

He totally is.

The program installed, I ran it to make sure it worked, and moved on to the final step:

`$ alias ls='sl'`

For the unitiated, the `alias` command creates a new command out of whatever existing commands you tell it. For example, I have an alias to save me from typing `ruby -run -e httpd . -p 8000` every time I want to run a simple server; instead now I just type `rubyserver` et voilà.

`ls` is a command that lists the files in the current directory and is such a common command to type, it's practically a tick for most programmers (up there with `git status` amirite). Meaning, it wouldn't be long before he discovered what I'd done.

There was one giveaway, of course: all that typing I'd done on his terminal. The best I could do since I had not opened a new terminal window (rookie mistake!) was type `clear` which does what it sounds like: leaves a blank terminal screen (session intact).

Then back to his Keyboard Preferences to delete the Canadian English keyboard. Thanks goes to [Nat Welch](http://www.twitter.com/icco) who helped with the keyboard nonsense and announced "Mischief Managed!" when it was all through, much to my delight.

Then I packed up my stuff to go home and even passed Steven coming back up the stairs on my way down.

I left out one important part of the story. What does `sl` do exactly?

Once installed, typing `sl` (or `ls` if you've mischievously aliased it) causes this to happen in your terminal window:

![mischief!](http://f.st-hatena.com/images/fotolife/n/noromanba/20141010/20141010042017.gif)

It's a totally benign program that rolls an ASCII steam engine across your screen. Cute, right?

But think about what I could have done. What if I were, in fact, A Monster?

Here Be Monsters
================

On an unlocked, unsupervised computer I've got access to all a user's files, their public and private ssh keys, their github account. Browser open? Chances are I've got your email too, and your Facebook account, and your Twitter account, and anything else you happen to be logged into.

I could send LinkedIn invitations to your entire address book.

What if you've got sensitive company or user data on your machine? I've got that now too.

But I'm not a monster, and Recurse Center is (as well it should be) a safe place to accidentally leave your computer unlocked. On the other hand, *out there* (aggressive handwaving), there be monsters, people quicker and better than me with malicious intent.

Steven's a good sport and a great guy, and it's one of the reasons why I knew I could pull off this little prank. It was less about teaching him a lesson and more about seeing with my own eyes just how much I could do on an unsecured laptop.

Now that I know, I'm more than a little horrified and surprised at *just how easy* the entire operation was. **It took maybe a minute, tops.** I remember the security engineers I worked with being accused of paranoia.

Something something it's not paranoia if it's true?

I think you can still live in a world where you can trust your friends, and even 99% of the people you meet, to not fuck with your shit. But that doesn't mean you don't lock your house or your car. There's valuable things in there -- your things -- and philosophical arguments about property aside, you lock them up to keep them safe from people who might want to take them. Computer security is no different.

Next time I'm signing Steven up for Candy Crush and inviting all his Facebook friends to play.

![eeeeeeeeeee](http://media.giphy.com/media/12lCxd2fqL2fL2/giphy.gif)

*Special thanks to [Franklin](https://twitter.com/thisisfranklin) for telling me about the whole `sl`/`ls` thing in the first place. <3*

[^0]: It's my understanding that other places are more likely to call this team ops or devops or some such? idk, like i said, i've only ever worked at the one tech company.