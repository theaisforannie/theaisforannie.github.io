---
layout: post
title: flatterybot for your phone!
---

The first program I ever wrote was a little bot called [Flatterybot](https://github.com/raybejjani/slackbot)[^1]. She's a sweet little thing who'll give compliments to anyone who asks.

Flatterybot is very simple. Held tightly to her heart is a YAML file of compliments. When a compliment is requested, she chooses one at random and sends it out. This simple but delightful output makes her easy and desirable to implement when I'm learning a new computer thing. For example, here she is as a [no-frills Sinatra web app](https://pacific-bayou-4823.herokuapp.com/).

So when I wanted to test out Twilio, as my first and favorite, she was the obvious choice:
![Now you can text Flatterybot!]({{ site.url }}/assets/sm_flatterybot_phone_screenshot.png)

As you can see, Flatterybot doesn't mind much what you say to her, but she's always got something nice to say[^2]. <3

You can text Flatterybot at 1 (305) 964-8649. Standard messaging rates apply, etc., etc., etc.




[^1]: Why is it not in my github? Flatterybot was written for the Slack team at Stripe -- when I left to go to RC, I transferred ownership of the bot to an engineer to ensure its continued maintenance and operation inside the org. I encourage you to clone the repo and add her to your Slack team too!
[^2]: Do be kind to her though: I can read the logs.