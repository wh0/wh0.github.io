---
date: Wed Oct 06 2021 23:44:22 -0700 (Pacific Daylight Time)
qualitative_time: 
title: User identity leak on Glitch
previous_teaser: How much peanut butter I can eat in an online meeting without people noticing
previous_first: true
---
In 2020, I discovered a way to identify Glitch users visiting a webpage.
This issue is now fixed.

It turned out that your Glitch user identity is not entirely private while browsing around the web, but the Glitch team still took steps to fix this issue.

## Copy of the report

Here's a copy of the report I sent to the staff.

Proof of concept:
[https://cdn.glitch.com/e74e582a-f676-47bb-a096-cf11029d2eb8%2Fasset-sc.html?v=1588023717826](https://cdn.glitch.com/e74e582a-f676-47bb-a096-cf11029d2eb8%2Fasset-sc.html?v=1588023717826)

It had recently (when I opened this) been shown that users can upload
HTML files as assets and have them served as pages, with full
client-sides scripting privileges. With these pages being served on a
subdomain of glitch.com (rather than glitch.me), they have some level
of access to read and write cookies shared with the main domain
glitch.com. While most of Glitch is prudent enough to use other
systems like LocalStorage, some third party libraries use cookies.
Among the information that leaks this way is the logged in user ID,
under the "ajs_user_id" cookie. The "ajs_" prefix seems to be
associated with the Segment analytics code.

## Notes

The proof of concept doesn't work anymore, thanks to changes Glitch made to fix the issue.

## Timeline

**2020/04/27**
@RiversideRocks [demonstrates](https://support.glitch.com/t/some-things-you-might-not-know/22838/4?u=wh0) that an entire website can run from the assets CDN.
I develop a proof of concept for this attack based on this technique.
This was during Glitch's transition to HappyFox, so I ask if I can report security issues there.

**2020/06/05** 
I complain on their support forum about the lack of response.
(Cross reference with the timeline of [another report](/2020/08/10/glitch-privesc-2019.html).)
They say that they replied and show me how to view my tickets online in case the notifications were being marked as spam.
However, viewing my tickets online reveals that tickets are visible to the public.

**2020/06/11**
By now, the ticket system is password-protected, and I submit the information about the vulnerability.
This time, I'm able to see them acknowledge it.

**2021/05/27**
They notify me that a fix is in place.
I check my proof of concept page, but it still works.
I find that they modified the asset upload system not to allow HTML files anymore.
However, HTML files uploaded before are still online.
Moreover, similar file formats such as XHTML can still be uploaded.
Moreover, the mechanism to disallow HTML uploads is implemented on the client side.
Moreover, the mechanism to disallow HTML uploads only shows an artificial error after the file uploads successfully.

**2021/10/02**
[@foxcorn](https://glitch.com/@foxcorn) remarks that their project named `cdn` is renamed by Glitch staff.

**2021/10/04**
By now, CDN resources redirect to their new cdn.glitch.me domain.
I confirm with staff that they've fixed this.

## Glitch's fix

They've moved the CDN to cdn.glitch.me, which doesn't share cookies with glitch.com.

I didn't later check if they changed the HTML upload prevention.

## Discussion

### Related: Watching editor presence

I had mentioned that your Glitch user identity is not entirely private while you're browsing around the web.
A few months after I reported this vulnerability, I found a seemingly intentional way to identify Glitch users visiting a webpage.

[Demo](https://island-foamy-paperback.glitch.me/).
Fair warning: it runs immediately.
Feel free to read the summary below instead.

Glitch allows websites to embed a project's source code editor/viewer.
When you connect to the editor, Glitch publicly shows that you're viewing it.
These together make it possible to embed a project source editor and run a program to watch as users show up on the editor.

The attack described in this article is slightly stronger in some ways though.
Watching the editor presences alone is detached from web client sessions.
This attack reveals the user identity in a specific client session, which makes it easier to tell who's who.
