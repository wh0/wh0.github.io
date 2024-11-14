---
date: Wed Nov 13 2024 20:37:48 -0800 (Pacific Standard Time)
qualitative_time: 
title: Backing up your Firefox Nightly profile on Android
previous_teaser: Substances that are liquid but that are considered dry
previous_first: false
has_highlighting: true
---
It's the same trick as [last time](/2015/11/10/fennec.html).

Download [v1.0.1](https://github.com/mozilla-mobile/fenix/releases/tag/v1.0.1).

Install it.

```sh
adb install -r -d firefox_preview-1.0.1.aarch64.apk
# don't launch this early version
```

Back up.

```sh
adb backup -f fenix-backup.ab org.mozilla.fenix
```

Then update back to the current version.

---

To restore, install the old version that allows backup, restore the backup, then install the 

```sh
adb install firefox_preview-1.0.1.aarch64.apk
# don't launch this early version
```

Restore.

```sh
adb restore fenix-backup.ab
```

Then update back to the current version.

---

If you need to extract something, some guy on the Internet said that, as long as you don't set a password, it's a custom 24-byte header followed by zlib compressed tar file.

```sh
tail -c+25 fenix-backup.ab | openssl zlib -d | tar -xv
```

I also see people saying their openssl doesn't have zlib, and they have [various other ideas](https://unix.stackexchange.com/q/22834) how to decompress it.

---

Interesting that Fenix had a version that allowed backup, years after they disabled backups in its predecessor, Fennec.
But whatever, the change is this commit [referencing some unseen force that restores the backup](https://github.com/mozilla-mobile/fenix/commit/badf79da48222b0249732f9a8f8c257875ee8d18).
GitHub indicates that this change is present all the way back to v1.1.0-rc.1 (at this time the versions must have been in a separate continuum from the rest of Firefox), so that's how I found v1.0.1 without it.
There were only ever 5 builds before this change, so we're lucky they exist at all.

Users on beta and release channels I think have no such luck.
