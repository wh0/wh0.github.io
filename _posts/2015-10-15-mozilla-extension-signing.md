---
date: 2015-10-15 00:26:23
title: Extension signing requirements for Firefox
previous_teaser: Spray bottle trigger discipline
previous_first: false
---
[The case  for extension signing](https://blog.mozilla.org/addons/2015/04/15/the-case-for-extension-signing/)
is that it prevents users from unwanted add-ons.
These add-ons alter web content, and they can violate individuals' security and
privacy.

To prevent it, they're going to make Firefox check for Mozilla's signature on
extensions.
The main challenge for them is going to be determining whether an extension does
these bad things, which isn't easy.
I'm not saying that because it's hard to analyze programs in general.
It's because the behavior of an extension depends not only on its code, but also
its configuration.

To make things more concrete about what extension configuration is, consider the
[Greasemonkey](https://addons.mozilla.org/en-US/firefox/addon/greasemonkey/)
extension.
It lets you run your own JavaScript code, called "userscripts," on web pages.
In this case, the set of userscripts is the configuration.
You don't know what Greasemonkey will do to a web page unless you know what
userscripts are in place.

I wonder what Mozilla will do about extensions like that.
Maybe they'll start signing userscripts too.
Maybe they'll disallow any extensions that depend too much on configuration and
tell people that userscripts ought to be complete extensions.
Maybe they'll sign Greasemonkey because it's really useful and popular, and end
up not improving Firefox security at all.
Or maybe I'm wrong about everything and this isn't even a problem.
