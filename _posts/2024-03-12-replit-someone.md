---
date: Tue Mar 12 2024 23:11:01 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Having Someone in your Replit editor
previous_teaser: A cool app that does something you desire but it also wastes 1% battery every minute it's on screen
previous_first: true
---
When Replit recently made advancements in its "deployments" system, they also made their older web hosting system effectively unusable.
Leading up to this change, you could host a website in a "repl" by running an HTTP server, and Replit would start the repl when someone loaded the website and shut it down a while later when there's no more traffic.
Starting in January 2024, the web server would instead be, as a staff member described it, "only be accessible when someone is in the editor."

So if you really didn't want to retrofitting your project to work as a deployment, you could just leave your editor open on your computer.

... Or, you could automate leaving the editor open, like how users automated making requests to keep their webservers up before this change.
Unusual for a service to allow that, right?
Yet they have a blog post welcoming that automation, as well as a special exception in their terms of service to permit it, as long as it doesn't create requests faster than you could mash F5:

> In using the Services, you may not and may not allow any third party, including your end users, to:
>
> 14\. Launch any automated system, including without limitation, "robots", "spiders", or "offline readers" that access the Service in a manner that sends more request messages or any other content to the Replit servers in a given period of time than a human can reasonably produce in the same period by using a conventional online web browser.

You wouldn't even have to reverse engineer, as they have published open source libraries for interacting with the editor's backend.

It all looked welcoming enough that I, for one, tried.

---

[(The try)](https://github.com/wh0/replit-someone)

It worked well, and the APIs and libraries worked as documented.
I published my findings.

---

Excerpt from the post:

> ## Experimental results
>
> I've tested this for the past several days.
> My repl stays up for however long it feels like (median ~2 hours), going down for usually a few seconds in between (median ~7 seconds).
> It was up for 99.90% of the time overall.
>
> Uptime/downtime CDF charts (warning: light theme lmao sorry)
>
> ![](/assets/2024/replit-someone-up.png)
>
> ![](/assets/2024/replit-someone-down.png)

---

Then I got this email (10 minutes after publishing):

> Hi Banned User,
>
> You have been banned from using Replit for this reason: "Violation of Terms of Service; account automation, reverse-engineering the service".
> If you believe that there has been a mistake and/or would like to appeal, you can reach out to us at appeals@replit.com.
> Please use the email address associated with this account when requesting an appeal.
> You can read more about our rules and community guidelines at [https://docs.replit.com/legal-and-security-info/trust-and-safety](https://docs.replit.com/legal-and-security-info/trust-and-safety).

So now all my projects on there are deleted.

That may have been a good, rational thing for them to do, banning a user that's using resources on their free plan.
But I reached out to the email address in the message anyway.
There are clearly some clerical mistakes in what they believe the terms of service to permit compared to what the version on their website permits.
And with the effort their team has taken to maintain SDKs and API documentation that made this project possible, I don't like them trivializing that by calling this project reverse engineering.

It's been three weeks since they confirmed having received my appeal request, and there's been no reply.

---

If you have to use Replit, follow your common sense over their official communications.
You can't just do what some random page called the terms of service says you can.
Oh, and back up your stuff.
