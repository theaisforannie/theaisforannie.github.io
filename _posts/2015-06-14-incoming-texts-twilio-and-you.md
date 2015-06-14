---
layout: post
title: incoming texts, twilio, and you -- a how-to guide
---

So you've signed up for twilio and you'd like to build an app that will customize responses to incoming text messages. Unfortunately, you, like me, find twilio's api docs to be maddeningly opaque. _Fortunately_, here's the workaround I figured out, courtesy of a lesson in HTTP requests from my friend [Kamal](http://kamalmarhubi.com/blog/).

Before we begin, I'm glossing over Sinatra, Heroku, and Twilio's sign up and configuration. If there's anything I wish tutorials would do is let you know what they're skipping over, so there, I just did.

Once you've signed up for Twilio and gone through that whole rigamarole, you can follow their [basic tutorial on responding to test messages](https://www.twilio.com/docs/quickstart/ruby/sms/hello-monkey). Your code will look something like this, only I hope with less extreme indentation (idk why it does that, that's not how i wrote it):

{% highlight ruby %}
require 'rubygems'
require 'twilio-ruby'
require 'sinatra'

get '/' do
	twiml = Twilio::TwiML::Response.new do |r|
		r.Message "Hello World!"
	end
	twiml.text
end
{% endhighlight %}

In the above, when someone texts your Twilio number, the app texts back a basic "Hello World!" message. This is pretty exciting! You are flush with power and decide the cool thing to do now is write a little program that varies its response based on the text that was sent to it! BUT HOW.

Again, this tutorial doesn't use Twilio's API because I can't figure it out. What I learned is more interesting anyway.

When you text a Twilio number, it contacts the server (in my case, Heroku) that you've set up. Heroku gives their URLs charming little names like "obscure-bayou-1234.herokuapp.com" so let's pretend that's our server address. Anyway, so Twilio goes to `GET` a response from obscure-bayou-1234.herokuapp.com/ (see the `'/'` between `get` and `do`? That's the same slash as at the end of our URL). It runs the little bit of ruby code there and responds with "Hello World!" like we told it to.

Now the cool part: there's more behind that `GET` request than you can see. Go ahead and run `heroku logs` on your command line:

	2015-06-14T19:31:40.163207+00:00 heroku[router]: at=info method=GET 
	path="/?ToCountry=US&ToState=TX&SmsMessageSid=fAkEnUmBeRsGoHeRe&NumMedia=0
	&ToCity=AUSTIN&FromZip=94956&SmsSid=fAkEnUmBeRsGoHeRe&FromState=CA
	&SmsStatus=received&FromCity=SAN+FRANCISCO&Body=foo+bar&FromCountry=US
	&To=%2B15124027511&ToZip=78736&MessageSid=fAkEnUmBeRsGoHeRe
	&AccountSid=DifferentFakeNumberGoesHere&From=%2B14155551234
	&ApiVersion=2010-04-01" host=obscure-bayou-1234.herokuapp.com 
	request_id=AnOtHeRfakeSetOFNumBErs fwd="11.111.111.111" 
	dyno=web.1 connect=1ms service=549ms status=200 bytes=397

It's gonna spew a lot of information at you, but you should see something that looks like the above. (I've changed or anonymized the actual data.)

There's a lot of stuff here, but we're just gonna focus on a bit of it. Notice first `method=GET` -- this is what kind of request is being made. _Then_ see what comes immediately after, `path="/?ToCountry=US&ToState=TX ..."` First of all, it starts with our friend `/`, then there's a question mark which indicates a query, then there's a bunch of gobbledy gook like `thing=otherthing&newthing=othernewthing`. _Those_ are the parameters of the request being made. Each parameter is separated by an ampersand `&`, so let's read it:

* `ToCountry=US`: the number the text was sent to is a US number
* `ToState=TX`: it's a Texas number
* `SmsMessageSid=fAkEnUmBeRsGoHeRe`: the unique id of the sent message.
* `NumMedia=0`: the number of media files in the message (e.g., an image). In this case there were none.
* `ToCity=AUSTIN`: not only did this number originate in Texas, but specifically Austin.
* `FromZip=94956`: the zip code of the message's _sender_[^1]
* `SmsSid=fAkEnUmBeRsGoHeRe`: this is the same as the `SmsMessageSid` above. It'll repeat itself one more time before we're done. (I actually don't know why.)
* `FromState=CA`: the sender's number originated in California!
* `SmsStatus=received`: cool, the message was received
* `FromCity=SAN+FRANCISCO`: the sender was me, btw. I'm from San Francisco. Hi!
* `Body=foo+bar`: and here's the titillating message I sent!!!
* `FromCountry=US`: my number originates in the US
* `To=%2B15124027511`: the real, non-anonymized number I sent "foo bar" to. I'll be blogging about this sooooon! Meanwhile, go ahead and give it some love. Just leave out the `%2B` part before the number. I don't know what that's about.
* `ToZip=78736`: an Austin zip code, apparently.
* `MessageSid=fAkEnUmBeRsGoHeRe`: ahh, the final appearance of our unique message id. (Not so unique after all, i guess. wah wah)
* `AccountSid=DifferentFakeNumberGoesHere`: my twilio account sid. You can't have it.
* `From=%2B14155551234`: the number that sent the message! Not my real phone number!
* `ApiVersion=2010-04-01`: the version of the Twilio API that we're skirting.

That's it for the `path` variable in the log. But HOLY COW every time someone texts your Twilio app, allllllll this information is included in the request. Only want to respond to people from San Francisco? You can do that now. Want to send location specific information to people with your app? You can do that too. Want to send admonishments to people who text you images because that costs more with Twilio? NOW YOU CAN.

And it's great, because that part is actual programming. The rest of this shit is just figuring out how other people's programs work so you can work with them.

Anywayyyyyys, all these parameters are made accessible to you, our weary but determined programmer, because Sinatra shoves them all inside its `params` hash. And just like any hash, when you call a key, you get a value.

Let's pretend you've written a little ruby script that turns strings to pig latin. When people text your Twilio number with some words, your app returns them in ig-pay atin-lay.

{% highlight ruby %}
require 'rubygems'
require 'twilio-ruby'
require 'sinatra'

def pig_latin(str)
	# your code here
	return ing-stray
end

get '/' do
	# Remember your code won't run unless it's inside 
	# the `get '/' do` block, but it's cool to define 
	# functions elsewhere.

	# `params['Body']` finds the message!
	# ^^^ THIS IS EVERYTHING YOU'VE BEEN READING ABOUT
	# FOR THE LAST COUPLE MINUTES
	str = params['Body'].downcase

	# Twilio code courtesy of Twilio since only
	# Twilio knows Twilio's magical incantations
	# This is from their tutorial I linked to above
	twiml = Twilio::TwiML::Response.new do |r|
		r.Message pig_latin(str) # look! our function!
	end
	twiml.text
end
{% endhighlight %}

And that's how I learned about `GET` requests and parameters and how it's helpful to have smart friends (thanks, Kamal! <3). This is the first time I've learned about this stuff *and* the first time I've attempted to explain it -- please report all inaccuracies to me on [twitter](http://www.twitter.com/anyharder). I'll blog next about what I made because it's the culmination of all this up here ^^^^ and it's silly.

[^1]: A _person_ might list all the `To` stuff together and all the `From` stuff together, but hashes aren't people, and they have no need for you or your meaningful way of ordering things. ¯\\\_(ツ)_/¯



