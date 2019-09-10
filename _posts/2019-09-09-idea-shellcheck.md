---
date: Mon Sep 09 2019 21:55:36 -0700 (Pacific Daylight Time)
qualitative_time: 
title: How Intellij IDEA installs shellcheck
previous_teaser: When volume 19 is loud enough
previous_first: false
---
Whatever it does, you end up with a statically linked binary in `~/.IdeaIC2019.2/config/plugins/Shell Script/shellcheck`.

Intellij IDEA 2019.2 introduces a new first-party plugin, Shell Script.
When you're using it, it'll put up a banner asking you if you want to install shellcheck to verify your shell scripts.
That's all you get, and then you have to decide between "Install" and "No, thanks."

"No, thanks" is like taking the blue pill.
It'll never ask you again, and you'll belive whatever you want to believe about how robust your shell scripts are.
So there's definitely some pressure to click "Install."

What happens when you click "Install" there?
I mean, practically, as the sysadmin of your own computer, you can't let them do whatever, right?

Will it install shellcheck with your distro's package manager?
Will it set up Brew, use Brew to install Haskell, and use Cabal to install shellcheck?
Will it `curl | sh` a script from somewhere?
Will it add a JetBrains employee's public key to your authorized_keys file so they can SSH in and clone the source repo and compile shellcheck with optimizations specific to your machine's CPU features?

And before you read this article, you'd wonder:
after they compile it, will they put it in `/usr/local/bin`?
Or `/opt/shellcheck/release`?
Or in the root of your home directory, because they don't have your sudo password?

Apparently it
https://github.com/JetBrains/intellij-community/blob/e17770110cb4b5e6fb5c34461bb8af04ef0815f4/plugins/sh/src/com/intellij/sh/shellcheck/ShShellcheckUtil.java#L89-L100
downloads a prebuilt version in a tarball from Bintray over HTTPS, extracts it, and it makes it executable.
Well that's not too bad.
