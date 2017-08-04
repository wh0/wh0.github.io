---
date: Thu Jan 05 2017 20:07:16 -0800 (PST)
qualitative_time: 
title: Cloud services, winter 2017
previous_teaser: Disposable garbage cans
previous_first: true
---
If you've come to this page already knowing what you want, skip straight past this introductory prose.
I arrive at sites like that all the time.

Let's take a look at some cloud services that you can get for free.
Each one has some important shortcomings, which I'll compare in this post.
It's understandable---the companies that provide these services want to encourage users to upgrade to paid offerings.
As you read, think about how to use these services together to complement a single one's limitations.
Also think about those spikes and armrest-like things you see on benches to keep people from sleeping on them.

To be more specific, this post is about getting someone else's computer to do what you want.
You're gonna need your own computer to communicate with that someone else's computer, and your own computer is probably capable of anything you want, but there are still reasons to use somoene else's computer.
Your computer might not have the right operating system to do what you want.
Or it doesn't have the right software installed.
Or it's just not on all the time when you want something done.
Yeah, there are lots of benefits using a cloud service.

You can use most of the ones in this post right now, but I've also included Koding Solo, which is now defunct, for comparison.

## Criteria

* Turns on / Turns off:
These free services don't let users have something running all the time.
They use some system to turn users' virtual machines on and off to save resources.
For services that turn off your VM automatically and you have to turn it on manually, you can use them for development, but it won't be useful for hosting.
It'll be easier to host an application on a service that'll automatically turn on your VM (although those tend to be unsuitable for development for other reasons).
* Network:
You always get to make outbound connections.
You'll probably want to accept incoming connections.
Some services only allow you to handle HTTP requests and not general IP traffic.
And realistically, this limitation probably won't be the only obstacle to deploying a non-HTTP internet application.
* Disk:
That's how much you can store.
And sometimes it's ephemeral, meaning it won't persist across boots of the VM.
* Root:
I find that it's a lot easier to get dependencies set up when I have root access.
You can feasibly prepare something to run without root for hosting, once you know exactly what you need.
It's way too annoying to try to do development without root access though.
* Container:
Even if you have root access, some exotic things don't work in containers (as configured in these cloud services, at least).
For example, you might not be able to use loop devices in a container.
This only comes up in a few of my projects.
I would think a lot of users never even notice.

## [Heroku](https://www.heroku.com/pricing)
This is my current choice for free web application hosting.

* Turns on: on traffic, up to 550 hours per month
* Turns off: after no traffic for 30 minutes
* Network: routed HTTP on single port, HTTPS, subdomain
* Disk: small (300 MB) "slug", plus ephemeral disk (I didn't check how much)
* Root: no
* Container: yes

## [Koding Solo](https://www.koding.com/blog/2016/03/goodbye-koding-solo-welcome-koding-for-teams/) (defunct)
I used this for development when it was still offered.
I was impressed with how generous the offering was.
They had a problem with abuse, and had to shut it down.
One weird thing about it was that every time you turned on your VM, you could get a different CPU.
For a while I couldn't figure out why something I compiled one day wouldn't run the next day.

* Turns on: when you turn it on
* Turns off: after you idle (I don't remember how long. A fraction of an hour.)
* Network: full IP, subdomain
* Disk: small (3 GB)
* Root: yes
* Container: no

## [Codeanywhere](https://codeanywhere.com/pricing)
This is my current choice for free development environment.
There are a few limitations with the size of the disk, the HTTP-only inbound connections, and the use of containers, so I can't do everything on it.
But it's still sufficient for a lot of things I do.

* Turns on: when you turn it on
* Turns off: after you leave
* Network: routed HTTP on any port, HTTPS, subdomain
* Disk: small (2 GB)
* Root: yes
* Container: yes

## [Dply](https://dply.co/how) (beta)
This is a new service.
They give you a VM that you can use for 2 hours.
You can immediately get a new one after your current one expires, but you'll lose any work that you didn't save outside the VM.
This is good, because it somehow feels unwholesome to "break in" to CI servers.
I use this to do things that don't fit on Codeanywhere's disk, or that needs weird kernel stuff, or that needs non-HTTP incoming connections.

* Turns on: when you turn it on
* Turns off: 2 hours later
* Network: full IP
* Disk: ephemeral, large (20 GB)
* Root: yes
* Container: no

## [Gomix](https://gomix.com/help/faqs/) (beta)
I only recently signed up for this.
I actually had a whole thing written up about how impressive the underlying offering was, despite how absolutely everything human-facing about the service was terrible.
But now that I think about it, I figure that my discoveries about the flexibility of the service could be unwelcome.
It could be that the flexibility is temporary, unintentional even.
For now, I'll take it as an online Node.js development environment with some incidental philosophy on how developers learn.
I'm keeping an eye on this though.

* Turns on: on traffic
* Turns off: after no traffic for 5 minutes
* Network: routed HTTP, single port, HTTPS
* Disk: small (128 MB, 100 files)
* Root: no
* Container: yes

## Special mention: [Teleconsole](https://www.teleconsole.com/)
You download a tarball, there's a static binary inside, you run it, it prints a URL, and you get a terminal running in your browser.
Now you can interactively control the computer where you ran that binary.
They proxy the connection so that you don't need to set up any port forwarding.
With that, you can get your precious shell without root (don't use their entire install script), without any incoming network connections.
