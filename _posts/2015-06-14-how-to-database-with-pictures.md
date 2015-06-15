---
layout: post
title: how to database, with pictures! <3
---


I was talking to [Julia](http://www.jvns.ca) this morning, who is capital-G Great btw, and I think I was being a little crabby and/or self-defeatist about my progress or lack thereof or (possibly) perceived lackthereof at Recurse Center. 

Because aw geez I am going into Week 4 and I don't even know how to database yet. I waved my arms around a bunch as I am wont to do and said a lot of words all at once, words like "what even *is* a database?", "how do i make one?", "is this one of those things where someone else already made it for me", "if so, where do i find the thing they made?", "will it be hard", "how do i put information into a database", "what language is database", "does database know ruby", "can i _see_ the database?", "how does a webpage talk to a database", "how does a database talk to a webpage", etc. In my memory we walked a whole city block while I just flailed at her.

She is very patient. It's been since before I left San Francisco that I had a good flailing.

She answered all my questions, including the main one, the thing that makes me most anxious about computer things: getting different computer things to talk to other computer things. Because at this point, I ~can write a ruby. Or I can consult the ruby docs about how to write a ruby, and it goes pretty well. To my astonishment, I wrote a `for` loop the other morning as part of a discussion with someone else, while he was looking over my shoulder, and I just _wrote_ it, and it just _worked_. I thought about holding a ticker tape parade to commemorate the occasion, but like I said we were in the middle of a thing.

I digress. I am mostly writing this because I want to remember the cool things that Julia told me, which were

Julia hits submit on form on my website
--> that triggers an HTTP POST request to my server
--> the server runs my code (in ruby) and returns a string which is parsed to be SQL code containing all the things Julia wrote on the form
--> the server sends the string to the database
--> which btw the database is both the database and the database application

Or perhaps you like pictures better, here I made these shitty pictures for you (actually for me):

![Some submits a form on catowners.com!]({{ site.url }}/assets/sm_databases1.jpg)

This is a website made of HTML and CSS and maybe some javascript, who knows. Catowners.com is your one-stop shop for keeping track of all your cats. Just fill in the form and hit submit!

But OMG the coolest part happens after you press submit. _It sends an HTTP POST request to the server for catowners.com!_ Like so:

![Let's run some Ruby!]({{ site.url }}/assets/sm_databases2.jpg)

Now we're running Ruby, finally something we understand. Whew! The Ruby parses the answers on the form and turns them into a string of the variety that a SQL database likes to read! Then the Ruby sends it to the database!

![the artist's rendering of a database]({{ site.url }}/assets/sm_databases3.jpg)

So one of my questions at this point was wait, the database holds the data, what understands the SQL? And according to Julia, the database is both the database AND the database application! Obviously this conception of databases is pretty high level and leaves out the nitty gritty -- I haven't picked a sqlite or a postgres off the shelf and taken it home for a test drive -- but this is good because I literally didn't have this understanding until this morning. Julia is the best!

(Confidential to nerds: I see that I listed Spot on the wrong row from Data, but you will deal.)

Here is a bonus picture of a robot with a circuit board belly that I started drawing and then realized I didn't know where I was going with him:

![beep boop ilu]({{ site.url }}/assets/sm_robot.jpg)