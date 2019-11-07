---
date: Thu Oct 31 2019 22:37:11 -0700 (Pacific Daylight Time)
qualitative_time: 
title: How IntelliJ determines if it can create a desktop entry
previous_teaser: The 999 islands that failed to revolutionize salad dressing
previous_first: true
---
I have Android Studio on a Debian computer, and it wouldn't allow me to use its "Create Desktop Entry" option.
And there's no precious diagnostic about why.
So here's why.
Android Studio is built on the IntelliJ platform, and there's an open source project, IntelliJ IDEA Community Edition, that's also built on IntelliJ, so we'll try to find out by looking through IntelliJ IDEA Community Edition's source code.

The [function that decides](https://github.com/JetBrains/intellij-community/blob/2676bd6deb8e98426ca9cdc0225eb093fee65069/platform/platform-impl/src/com/intellij/ide/actions/CreateDesktopEntryAction.java#L46) if the action is available considers:

- if your system would feasibly be running the X Window System; **and**
- if something called the "external update manager" considers the installation **not** to be "roaming;" **and**
- if you have `xdg-open`.

## X Window System

The [criteria](https://github.com/JetBrains/intellij-community/blob/2676bd6deb8e98426ca9cdc0225eb093fee65069/platform/util/src/com/intellij/openapi/util/SystemInfo.java#L79) for whether your system would reasonably be running the X Window System turns out to be a pretty simple heuristic:

- if your system is Unix; **and**
- if your system is **not** Mac.

The [criteria](https://github.com/JetBrains/intellij-community/blob/2676bd6deb8e98426ca9cdc0225eb093fee65069/platform/util-rt/src/com/intellij/openapi/util/SystemInfoRt.java#L15-L20) for whether the system is any of several possible options is determined from the `os.name` Java system property, and the Unix-ness is inferred from the system not being Windows.

I use Debian GNU/Linux, so that should mean my system would feasibly be running the X Window System.
Incidentally, I think I do, because I'm using a proprietary Nvidia driver that wasn't working right with Wayland or something.

## Not "roaming" installation

The "external update manager's" [criteria](https://github.com/JetBrains/intellij-community/blob/2676bd6deb8e98426ca9cdc0225eb093fee65069/platform/platform-impl/src/com/intellij/openapi/updateSettings/impl/ExternalUpdateManager.java#L46) for the installation to be "roaming" are:

- if the "actual" external update manager is "toolbox;" **or**
- if the "actual" external update manager is "snap,"

whatever it means for it to be those.

The "actual" external update manager, for the cases we're interested in, is inferred with [string matching](https://github.com/JetBrains/intellij-community/blob/2676bd6deb8e98426ca9cdc0225eb093fee65069/platform/platform-impl/src/com/intellij/openapi/updateSettings/impl/ExternalUpdateManager.java#L46) on the IDE's "home path":

- substrings `/apps/` or `/ch-` mean that the actual external update manager is "toolbox;" while
- prefixes `/snap/` or `/var/lib/snapd/snap/` on linux mean that the actual external update manager is "snap."

There's some big complex [algorithm](https://github.com/JetBrains/intellij-community/blob/2676bd6deb8e98426ca9cdc0225eb093fee65069/platform/util/src/com/intellij/openapi/application/PathManager.java#L139-L177) for figuring out the "home path".
To be clear, it's not the user's home directory.
It's some location specific to the IDE installation.
The algorithm starts at a path from the class loader and walks up the tree looking for a directory with a `bin/idea.properties` file.
Fancy.

I know I didn't use Snap to install Android Studio.
The only way to get the preview release is to download a tarball.
I was lucky I didn't extract it into a path including `/apps/` though.
So that should mean the external update manager considers the installation to be roaming.

[Toolbox](https://www.jetbrains.com/toolbox-app/) is apparently JetBrains's own software installer/updater program.

## `xdg-open`

I didn't look much into how it checks for `xdg-open`.
It's [whatever `PathExecLazyValue` does](https://github.com/JetBrains/intellij-community/blob/2676bd6deb8e98426ca9cdc0225eb093fee65069/platform/util/src/com/intellij/openapi/util/SystemInfo.java#L97-L100).

I checked in a terminal, and I didn't have it.
So that should mean that I don't have `xdg-open`, so that the action to create a desktop entry ought to be unavailable, which is consistent.
I installed `xdg-utils` and after that I was able to have it create a desktop entry.

It looks like [it uses another program](https://github.com/JetBrains/intellij-community/blob/2676bd6deb8e98426ca9cdc0225eb093fee65069/platform/platform-impl/src/com/intellij/ide/actions/CreateDesktopEntryAction.java#L158-L159), `xdg-desktop-menu`, that comes with `xdg-utils`.
In that case, I guess it's justified that they check for `xdg-open` as a proxy for whether `xdg-desktop-menu` is installed.

`xdg-utils` is reported installed in [55%](https://qa.debian.org/popcon.php?package=xdg-utils) of Debian installations.
I must have been in that weird 45%.
I found out today that Ubuntu recently updated their popcon data too.
It's installed on [92%](https://popcon.ubuntu.com/main/by_inst) of Ubuntu installations.
But that data also says [there's a lot more i386 installations than amd64](https://popcon.ubuntu.com/).
Hm.

## Additional comments

Here's the desktop entry it generated, in `~/.local/share/applications/jetbrains-studio.desktop`:

```
[Desktop Entry]
Version=1.0
Type=Application
Name=Android Studio
Icon=/home/wh0/software/android-studio/bin/studio.png
Exec="/home/wh0/software/android-studio/bin/studio.sh" %f
Comment=The Drive to Develop
Categories=Development;IDE;
Terminal=false
StartupWMClass=jetbrains-studio
```

I had extracted the tarball in a path that contained the substring `/software/`.
Really dodged a bullet on that `/apps/` check.

Anyway, I wrote this junk down so that we would have some evidence of why open source stuff is nice, even if you're stuck with a non-free downstream project.
There's stuff that isn't documented but that you would be able to look up in the source code.

Oh, and good on JetBrains for making a repo where there's only one obvious `.java` file for each class name you see.
And for not making everything an interface where you have to go searching for what implements it.
Could we all please hold off on that until web code browsing has intelligent cross referencing?
