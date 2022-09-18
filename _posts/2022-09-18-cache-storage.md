---
date: Sun Sep 18 2022 12:01:22 -0700 (Pacific Daylight Time)
qualitative_time: 
title: The uncoolness of CacheStorage
previous_teaser: People who turn doorknobs away from the door hinge
previous_first: true
---
I mean [this CacheStorage](https://developer.mozilla.org/en-US/docs/Web/API/CacheStorage), a browser API for saving HTTP-like Response objects indexed by HTTP-like Request objects.
The thing commonly used from a Service Worker.

Three: The class that you end up using, Cache, has a name so generic that it's ungoogleable.

Two: You can't put a pending response into a Cache, so you have to do your own coordination to prevent a cache stampede.

One: The storage management and intended use cases make it seem not to be a cache at all. From all I've read about Cache, data stored in is treated as preciously as IndexedDB. The browser will not help you clean out unused entries. It's just storage.

P. S., markdown auto numbering, you're killing me
