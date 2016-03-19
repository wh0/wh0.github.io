---
date: 2015-10-10 19:00:00 -0700
title: Meteor stuff
---
I've been working on some projects that reduce the disk space requirements of doing Meteor development.

[mongocrush](https://github.com/wh0/mongocrush) patches the Meteor tool to change how it runs MongoDB,
reducing how much disk space the development database takes.
It's mostly related to storage preallocation, so sites that actually have a lot of data will still use a lot of storage.
But it saves about 64 MB on an empty site.
In earlier releases of Meteor, it saved about 512 MB.

[meteorsquash](https://github.com/wh0/meteorsquash) is a repackaging of the Meteor bootstrap package,
which runs the included packages from a squashfs filesystem.
The compressed image reduces 580 MB to 74 MB.
