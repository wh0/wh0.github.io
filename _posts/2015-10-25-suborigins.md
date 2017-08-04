---
date: Sun Oct 25 2015 21:35:27 -0700 (PDT)
title: Relinquishing control with suborigins
previous_teaser: How a knife works
previous_first: false
---
How might a server deliver a web application and then relinquish control of the client-side code?
It's a niche problem, but I think the really paranoid people out there would appreciate a solution to it.

Think back to the closing down of Lavabit.
I assume that they had a cleverly designed system where confidential messages would only ever be decrypted on the client.
Somewhere in that case, we came to understand that such a system would be one court order away from being backdoored.
The server sends out the client-side code on a regular basis, and may someday send code that uploads the decrypted confidential messages of a particular user.

This rest of this article presents a design to overcome this kind of limitation.

---

## Overview

The main idea is to serve a page in an unpredicably-named suborigin and then forget the name of that suborigin.
The browser caches the response, durably binding the page to that suborigin, so that it can later be reloaded without asking the server.
Once the server forgets the name of that suborigin, the confidentiality and integrity of the client is safe from future meddling from the server.

After that, the same origin policy will keep the suborigin safe, right?
Not really.
There are a lot of problems still.

## Specification changes

Maybe in the future, people will decide that the main non-sub origin ought to have power over suborigins.
Or maybe there will be a way for an origin to enumerate its sub origins.
I don't know what will happen, and the team in charge of designing suborigins has committed to supporting a system like this.

In this design, the exact mechanism by which the page is cached is in limbo.
I've only just gotten it to work right with the HTML5 application cache, but by now, that feature is being removed.
Plus, there could be some as of yet unresolved conflict with the manifest technically coming from a different origin
(currently I'm serving the manifest without a suborigin, because remembering the name across two requests would <s>be a pain in the butt</s> significantly increase the complexity required for correct operation).
And I get the feeling they're not going to allow a suborigin to install its own service worker, because it would be, like, impossible to keep track of them.

## Suborigin name secrecy

The fact that the server has forgotten the name of the suborigin is only the beginning of the story.
The application needs to keep the name secret.
And that's hard.
The name of an origin is generally regarded as public metadata.

For example, any XMLHttpRequest request will send an `Origin` header (suborigins are never same-origin with a server, so all requests are treated as cross-origin).
And any postMessage message will include the sender's origin.
I don't think we can even know an exhaustive list of ways an origin can be disclosed.

Maybe this is easy enough to work around.
Just do all communications in a sandboxed iframe, which runs with a null origin.
Or maybe I'm missing something important and the origin still gets out if you do this.

## User awareness of suborigin

There's no availability guarantee of the page in the suborigin.
This design isn't a way for a server to send a page and completely hose that URL so that it can never touch it again.
Since there's no high-assurance caching mechanism, the server can at any time redeliver the page (although the new page would be in a new suborigin, or no suborigin).

That means that, while the server can't directly snoop on the origin that it forgot,
it can do a really good job of phishing if the application doesn't take the right precautions.

An application can defend itself against this by displaying its suborigin.
Or a fingerprint derived from its suborigin.
Or another unpredictable value that the server also forgets.

The user will have to verify this display before interacting with the page (e.g., entering a password).

---

That's all the various security considerations I can think of for now.
That is, maybe there are some other major problems with this design.

[Example application code](https://github.com/wh0/tofustrap).

[Live demo](https://tofustrap.herokuapp.com/).
Not that suborigins are currently specified or implemented.
There's a prototype that ships with Chrome though.
You can enable it by running Chrome with `--enable-blink-features=Suborigins`.
