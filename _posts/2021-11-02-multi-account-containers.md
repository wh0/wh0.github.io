---
date: Tue Nov 02 2021 10:03:11 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Firefox Multi-Account Containers now needing permission to "Exchange messages with programs other than Firefox"
previous_teaser: 
previous_first: true
---
A new version of [Firefox Multi-Account Containers](https://addons.mozilla.org/en-US/firefox/addon/multi-account-containers/) requires permission to "Exchange messages with programs other than Firefox."

![](/assets/2021/multi-account-containers-permission.png)

Have a look at the two vague lines in [the release notes](https://github.com/mozilla/multi-account-containers/releases/tag/8.0.0):

> Now Containers can integrate with Mozilla VPN or your own proxy to create a secure, private connection that protects your activity.
>
> ...
>
> * ✨ MozillaVPN Integration ✨ by @lesleyjanenorton in #2157

I tracked this down to [a PR in the extension's source repository](https://github.com/mozilla/multi-account-containers/pull/2157).
From that PR:

> If the Port [a connection to the Mozilla VPN native app] is established, the MAC [Multi-Account Containers] interface will be updated with a slew of MozillaVPN specific features that facilitate painless per-container proxy configuration.

🙁 It's been nice having this extension and a commercial VPN service being two separate things while it lasted.
