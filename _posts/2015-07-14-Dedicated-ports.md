---
layout: post
title: Dedicated Ports of the Internet
---

I've been reading [RFC 2616](http://www.ietf.org/rfc/rfc2616.txt), which if you haven't read it lays out all the MAYs and MUSTs and SHOULDs for using HTTP (and therefore, [the internet as we know it](https://medium.com/@maradydd/on-port-80-d8d6d3443d9a)).

HTTP is among the protocols designated by the IANA for a dedicated port. If you're running an HTTP server, you run it on port 80, and everyone knows to look for it there.

There are other ports dedicated to other protocols. I've implemented a few and had a lot of fun along the way, but first

What is a port?
===============
I've spent the last six weeks making mischief with servers, ports, and sockets, and I only just figured it out, so if this doesn't click right away, don't feel bad. To use the same analogy you see everywhere, 

<p><center>IP Address : Port :: Phone Number : Extension</center></p>

If you're an office, you have one main phone number and a whole list of extensions to assign to different people or departments. If you're a computer, you have one IP address and a whole list of ports to assign to different things. You can theoretically serve whatever you like on each of them, though that will get confusing for people who go looking for specific things on specific ports. Those specific ports are "dedicated."

What is a dedicated port?
=========================
Continuing the analogy, a dedicated port is like assigning Extension 250 to the CEO, Extension 100 to the IT department, Extension 443 to Security. Once you know who is at which extension, you know what to expect when you dial them. They've been dedicated.

So to with the internet, it turns out. The Internet Assigned Numbers Authority reserves [ports 0-1023 for particular uses](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers).

That sounds boring. Phone extensions are boring.
================================================
Well, you're wrong. It's actually kind of interesting and occasionally bizarre (but that's another blog post).

As we've discussed, you've got Port 80 running HTTP Servers, but also there's Port 22 for ssh and HTTPS on Port 443. IRC servers run on Port 194. SMTP runs on Port 25, and for all you nerds out there, you'll find servers running the Gopher protocol on Port 70.

How do I find one of these ports
================================
`netcat` can do it! You can scan the ports for a given IP address to see if there's anything running there with the `-z` and `-v` flags. `-z` will scan through the ports you request, `-v` is the verbose option (we want the output to be chatty).

`nc -z -v pchs.co 1-100` will scan through ports 1 through 100 on my remote server, pchs.co. Go ahead and try it!

The output (up to port 10 anyway) will look something like this:

	$ nc -z -v pchs.co 1-100
	nc: connectx to pchs.co port 1 (tcp) failed: Connection refused
	nc: connectx to pchs.co port 2 (tcp) failed: Connection refused
	nc: connectx to pchs.co port 3 (tcp) failed: Connection refused
	nc: connectx to pchs.co port 4 (tcp) failed: Connection refused
	nc: connectx to pchs.co port 5 (tcp) failed: Connection refused
	nc: connectx to pchs.co port 6 (tcp) failed: Connection refused
	found 0 associations
	found 1 connections:
	     1:	flags=82<CONNECTED,PREFERRED>
		outif en0

		dst 45.55.21.185 port 7
		rank info not available
		TCP aux info available

	Connection to pchs.co port 7 [tcp/echo] succeeded!
	nc: connectx to pchs.co port 8 (tcp) failed: Connection refused
	nc: connectx to pchs.co port 9 (tcp) failed: Connection refused
	nc: connectx to pchs.co port 10 (tcp) failed: Connection refused

Port 7 succeeded! _There is something there!_

I'm going to write another post about the servers I've got running on various ports (and the old protocols I implemented on them), but for now you're welcome to enter `nc pchs.co 7` on your command line to test out it out for yourself.

One more thing about ports
==========================
"If ports are like extensions, how come I never have to dial one?"

Great question!

When you run a simple server on, say, `localhost:4567` ([Sinatra](http://www.sinatrarb.com/)'s default port of choice), you see the host (`localhost`) followed by a `:` followed by `4567`, the port. How come you never see this anywhere else? How come you didn't have to type `blog.annharter.com:80` to get here?

A snippet from RFC 2616 (the one all about HTTP that I mentioned at the start):

	3.2.2 http URL

	   The "http" scheme is used to locate network resources via the HTTP
	   protocol. (...)

	   http_URL = "http:" "//" host [ ":" port ] [ abs_path [ "?" query ]]

	   If the port is empty or not given, port 80 is assumed. 

Gonna repeat that for effect:

	   If the port is empty or not given, port 80 is assumed. 

You can see this for yourself by running a simple server right from your machine. First, for didactic purposes:

`ruby -run -e httpd . -p 4000`

Navigate to `localhost:4000` in your browser, and depending on what kind of directory you're in, you'll probably see an index of files there. Note the address in the URL bar literally says "localhost:4000". Now break the connection (Ctrl + C) and change `-p 4000` to `-p 80` to rerun the command:

`ruby -run -e httpd . -p 80`

Unless you've got root access, you're going to get a `Permission Denied` error (modern operating systems prevent you from opening well-known ports on your machine willy-nilly). You can prepend `sudo` to the above command to try it again.

After you enter your password, go back to your browser and type `localhost:80` in the URL bar. What you should see after you hit Enter is not `localhost:80` but just ... `localhost`. Your browser runs by default on port 80 and strips away the extraneous information, but it turns out, _it's there_. Neat, huh?[^0]

It took me a really long time to understand ports, and I think this was largely due to the way the port we interact with the most, port 80, is hidden all the time. Next time, we'll visit a few ghost towns[^1] and maybe even learn something from one really boring client-server conversation.

_Thanks to [Shale](https://twitter.com/__shale__) for showing me the `localhost:80` thing._

[^0]: Don't forget to break the connection!
[^1]: s/towns/ports


