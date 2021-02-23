---
date: Mon Feb 22 2021 23:16:56 -0800 (Pacific Standard Time)
qualitative_time: 
title: Cloud services, winter 2021
previous_teaser: Power strips that take two hands to unplug something
previous_first: true
---
Here are some thoughts on the past four years of my top free services--hosting and online development environments--for tiny personal things.

## [Glitch](https://glitch.com/)
Glitch was called Gomix in the last post, but they've been able to get the Glitch name from Slack's cold, pivoted hands.

Since last time, they've added a limit to how long your projects can be online--1,000 hours per month;
they've thrown in the towel on letting users automate requests to keep their sites online; and
they've moved private sites to be a paid feature.
Oh, that is to say that they released a paid tier with, guess what?
More hours, always-on projects, and private projects.
One more thing: they added a "static site" classification of projects that lets them serve projects from a CDN without using up your hours, but you can't customize the 404 page, so it's not the best option for those of us who can handle GitHub Pages.

I'm still on this one, on the free tier, and I'm in deep.
The unlimited projects (as long as few enough actually run) and persistent storage put it above other offerings.
This is my top pick for any projects that entirely fit into my head.

## Red Hat hosted Eclipse Che
I never did really figure out the confusing constellation of names: Red Hat, Eclipse Che, Eclipse Theia, CodeReady Toolchain, CodeReady Workspaces, OpenShift ... .io.

This service was the result of Red Hat acquiring Codenvy in May 2017.
I didn't bring Codenvy up last time because it wasn't one of my top choices, due to some kind of monthly hour limit.
I had an account though, and they emailed me in November 2019 about this new service.
This offering had no monthly hour limit, and it went on to become my top pick for online development environment.

Che's Theia IDE is comparable to Visual Studio Code, including support for Language Server Protocol.
It's so familiar that you'll wonder if the Theia docs are being entirely truthful about having only taken Monaco and LSP from VS Code.
But you'll be reminded that you're not in VS Code when you see things like the inline Git diff viewer not working.
There are other editor frontends available through Che though.
Impressively, their installation of IntelliJ IDEA Community Edition worked fine, over some kind of in-browser X display.
The code-server (supposedly a browser-compatible port of VS Code) never worked for me though.

You don't get root access in the development container, but you can run whatever container image you want.
This would have gone nicely with a service that let you build images from Dockerfiles, although I wasn't using any such service while Red Hat was offering this.

It was slow to start up, and combined with the short turn-off-your-stuff-when-you're-intactive timer led to many cases where I would switch tabs, come back later, find that it finished loading and died due to inactivity, then had to start over.
It was also kind of unreliable, with some days not letting me log in and some days giving weird CephFS glitches making a file show up in a directory listing but otherwise appearing not to exist.

But as we speak, this service is being phased out and replaced.
See below.

## [Red Hat OpenShift Developer Sandbox](https://developers.redhat.com/developer-sandbox)
At least this one is clearer about what it's called.

It's a hosted Kubernetes cluster.
You'll need to verify a phone number.
They stop pods off after 8 hours, so it's probably not suitable for hosting.
They wipe your cluster after 14 days.
After that, you can sign up again.
I found out about this last part on [a live stream](https://youtu.be/oDqw8aBGDD8?t=561) that they ran.

![Luiz Almeida: How many times I can use the sandbox? Just once? / Red Hat Developer: sandbox can be use an unlimited number of times, it just gets destroyed every 14 days](/assets/2021/cloud-services-chat.png)

The in-browser IDE is still there, in a section called CodeReady Workspaces.
They also have a section for building your own container images, and these two seem to be great complements for one another.

You still don't have direct access to run as root in containers.
You run as root inside Docker builds, so I don't get why they have this restriction.
I just want to be able to install one more package temporarily without starting over in a brand new container every time.

## [Repl.it](https://repl.it/site/pricing)
They had been a service where you could test out snippets of code in a temporary container.
Among all the bells and whistles they have been adding, two stand out as relevant to this subject.
In January 2018, they added web hosting, and in September 2020, they let those websites have a database.

These additions started making Repl.it an appealing free hosting service, offering unlimited projects and unlimited time.
And notably, when Glitch threw in the towel on automated requests, referred to as "pinging" in their community, Repl.it moved to capitalize by going on the record to say that [they would always allow pinging to keep projects online](https://blog.repl.it/glitch).

> Because many people are asking us, as co-founders & CEO I want to make a promise to you that we will never block pinging services or make any abrupt changes that will break your apps.
>
> ...
>
> We have a new feature coming out for Hacker plan subscribers that will make repls [that's what they call the projects on their service] even more reliable that they wouldn't need pinging in the first place.
> But we will never ban pinging to force people to upgrade by taking away features.

Since that post, they indeed have added an always-on feature to their paid tier, and they've stuck to their guns on continuing to allow free project pinging.

But with all the generosity on paper and the bold messaging from the leadership, I haven't been able to get into this one.
There's too many weird things about it that I spent energy figuring out in the short time I was interested.
Some of these things perhaps come from the hosting feature being an afterthought.

- The disk is ephemeral.
- Except when you have the project open in the editor.
  Any changes during that are persisted.
- Except if the user went to repl.run instead of repl.co, which runs as a separate copy, which of course doesn't affect your project.
- You can't programmatically tell what mode the project is running in.

And then there are some things you have to get used to that are too Repl.it-specific for me to put up with right now.

- Project logs are exposed publicly.
- There's some reckless automated process that scans your project for imports and enters them into a project manifest (e.g. `require` calls to `package.json` `dependencies` for Node.JS).
- The database interface is a completely custom creation.

## Heroku
Heroku added a new container runtime, which might be cool for some people.
I hadn't mentioned this last time, but the dynos have a lot of CPU cores, which makes it a good host for bursty parallelizable work.
Still no sign of AVX2 ever though.

## Actual cloud provider: Oracle Cloud
During this time I broke down and signed up for some actual cloud proviers that have free tiers (time limited trials excluded).

You can use Oracle Cloud's free tier after signing up with a credit card.
You can get two always-on VMs, double the generosity from elsewhere.

## Actual cloud provider: Google Cloud
You can use their free tier after signing up with a credit card.
You can get a single always-on VM on their Compute Engine.

But any traffic from China costs money, and that's currently eating my free trial credits.
I wonder what I can do after those credits expire.

## Codeanywhere
Codeanywhere shed its free tier in February 2019.
They couldn't afford it anymore.

## Dply
Dply quietly disappeared when I wasn't looking.
Records from the Wayback machine put this some time between June and August 2018.

## Special mention: Teleconsole
Teleconsole quietly stopped working.
Their website is still up, but their installer script fails to find any suitable releases, and if you get the client manually, you'll find that none of the servers are up.

I wonder if I could build a self-hosted clone.

## Special mention: [Wonderfl](https://www.kayac.com/en/service/other/785)
Wonderfl was a development environment and host for ActionScript programs, which run in Flash Player.
They shut down in March 2017.
I scraped a copy of my own projects from it, but I never republished the projects anywhere.
Now Flash Player is gone too.
