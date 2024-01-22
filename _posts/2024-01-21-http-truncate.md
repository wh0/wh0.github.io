---
date: Sun Jan 21 2024 22:30:17 -0800 (Pacific Standard Time)
qualitative_time: 
title: Receiving a truncated HTTP response in Node.js
previous_teaser: Kitchens where you always cast a shadow on the exact thing you're working on
previous_first: true
has_highlighting: true
---
Here's the example, slightly reduced, from the Node.js docs on how to make an HTTP request using the built in HTTP API.

```js
const req = http.request(/* ... */, (res) => {
  // ...
  res.on('data', (chunk) => {
    // ...
  });
  res.on('end', () => {
    // (B) success
  });
});
req.on('error', (e) => {
  // (A) failure
});
req.write(/* ... */);
req.end();
```

Commendably, this example code even has error handling.
One would guess from the structure that if the request goes wrong, a callback (A) will run, or otherwise you'll get to another callback (B) after receiving the response successfully.

A question came up though, does code like this adequately handle network errors while reading the response body?

It doesn't; it silently never gets to the code in callback (B).
Except for if you're using old Node.js before 13.0, in which case it silently does get to the code in callback (B).

## What happens

The docs have an overview of what happens.
Go to [the section on `request()`](https://nodejs.org/api/http.html#httprequestoptions-callback) and search for "connection close after the response is received."
Or don't, right now it's off by a little
(I submitted a correction [#51464](https://github.com/nodejs/node/pull/51464)).

It varies by Node.js version though.
I've experimentally collected what happens through different versions and expanded it with how newer additions like `stream.pipeline` behave.

![](/assets/2024/http-truncate-events.svg)

_Order of events, related state changes, and callbacks, by Node.js version.
In elongated boxes, look for the highest version that's no higher than the version you're using.
The "d" circles indicate deprecated APIs, and the "e" circles indicate experimental APIs._

## History

There have been several tweaks to how the various events, state changes, and callbacks play out.

Early Node.js (I tested as far back as 4.0) started with emitting
a request `close` event,
a response `aborted` event,
a response `end` event, and
a response `close` event
in the case of a truncated response.
Using `res.pipe(dst)` calls `dst.end()`.

In 8.13, they added a response `aborted` property [#20094](https://github.com/nodejs/node/pull/20094), to be set to `true` along with emitting the `aborted` event.

👉 Aside on simplifying the chronology: I'm only going to write the lowest version that a change has been backported to.
In reality, for example, the response `aborted` property from above came during the 11.0 development cycle (released 2018 Oct 23).
It was backported to 10.1 (released 2018 May 9) and 8.13 (released 2018 Nov 20).
So all of the 9.x versions and 10.0 didn't have it.
The diagram doesn't capture this, to reduce how many back-and-forths I have to draw.

In 9.0, they moved the request `close` event to after the response `aborted` event [#15588](https://github.com/nodejs/node/pull/15588).

In 10.0, they added experimental async iterator support [#17755](https://github.com/nodejs/node/pull/17755) and the `stream.finished` and `stream.pipeline` functions.
At this point, all three of these treat the response as being received successfully, so they're not usable to detect a truncated response.

In 10.17, they marked async iterator support as stable [#26989](https://github.com/nodejs/node/pull/26989).

In 12.9, they added a response `readableEnded` property [#28814](https://github.com/nodejs/node/pull/28814).
At the time though, the response still ends (i.e. emitting an `end` event), so it's not usable to detect a truncated response.

In 12.10, they made `stream.finished` and `stream.pipeline` give you an error [#29376](https://github.com/nodejs/node/pull/29376) by internally detecting the `aborted` event.
This makes them usable to detect a truncated response.

In 12.18, they changed `stream.pipeline` to call `res.destroy(err)` [#30869](https://github.com/nodejs/node/pull/30869) instead of `res.destroy()` when shutting down.
Through various steps involving the socket that both the request and response refer to, this further causes a request `error` event.
However, Node.js crashes when there's an unhandled `error` event anywhere
(an `error` event thus differs from a tree falling in the woods.)
To avoid making existing programs crash now, the pipeline additionally adds a dummy `error` listener to make the event count as handled.

In 12.19, they changed `stream.pipeline` to call `res.abort()` [#31054](https://github.com/nodejs/node/pull/31054) instead of `res.destroy(err)`.
This causes there not to be a request `error` event, and it instead causes a request `abort` event.

In 13.0, they stopped emitting the response `end` event [#27984](https://github.com/nodejs/node/pull/27984).
This causes the response `readableEnded` property to remain false, which makes it usable to detect a truncated response.
This also makes `res.pipe(dst)` not call `dst.end()`.
The implementation also allows the response `close` event to be emitted earlier.

In 13.10, as part of a `stream.pipeline` feature addition [#31223](https://github.com/nodejs/node/pull/31223), they made the pipeline shutdown procedure call your callback right away without waiting for its streams to finish closing.

In 14.3, they started setting the request `destroyed` property [#33120](https://github.com/nodejs/node/pull/33120).
This property existed on the superclass (writable stream) since earlier, but it had been unused.
Don't use this to detect a truncated response though.
It's set even if you receive the complete response.

In 15.0, they started setting the response `destroyed` property [#33131](https://github.com/nodejs/node/pull/33131).
Similar to the above, the property existed on the superclass (readable stream) since earlier, but it had been unused.
Also similar to the above, don't use this to detect a truncated response either.
It's also set even if you receive the complete response.

👉 Additional aside on simplifying the chronology: I don't count it when a change is backported and then reverted.
In reality, for example, the response `destroyed` property from above was also backported to 14.3, but then it was reverted in 14.5.
The diagram leaves that part out.
This, too, reduces how many back-and-forths I have to draw in the diagram.

Also in 15.0, they started emitting a response `error` event [#33172](https://github.com/nodejs/node/pull/33172).
Here they use a different approach to avoid making existing programs crash.
The new code in this change only emits the event if there is a listener registered.
This change also causes async iterators to call `req.abort()` and throw; the former causes a request `abort` event.
And with the async iterator throwing, it's usable to detect a truncated response.

In 16.0, they reimplemented the way the response shuts down [#33035](https://github.com/nodejs/node/pull/33035), which re-ordered how several things happen.

Also in 16.0, they made `stream.finished` no longer count the response `aborted` event as finishing [#36649](https://github.com/nodejs/node/pull/36649).
This changes the error you get from it, from "Premature close" to "aborted."
`stream.pipeline`, which internally uses `stream.finished`, is similarly affected.

In 16.12, they deprecated the request/response `abort`/`aborted` events/properties [#36670](https://github.com/nodejs/node/pull/36670).
The response `complete` property was spared, but it was never written in the discussion why.

In 16.15, they added built in `fetch` [#41749](https://github.com/nodejs/node/pull/41749), which I consider pretty usable.

In 17.0, they made `stream.pipeline` and async iterators stop calling `req.abort()` [#38505](https://github.com/nodejs/node/pull/38505).
This prevents the request `abort` event.

In 17.3, they made some `stream.pipeline` change [#40881](https://github.com/nodejs/node/pull/40881) with an empty pull request description, a title that sounds unrelated to truncated responses, and code that looks buggy
(I reported my findings [#51540](https://github.com/nodejs/node/issues/51540)).
Anyway, part of that change causes `stream.pipeline` to call your callback later.

Nothing has changed since that
(I tested up to 21.6, which is the latest release as of now).

## Recommendations

If higher level abstractions are suitable, use `stream.pipeline` or `stream.finished`.
These correctly give you an error since Node.js 12.10.

```js
const dst = /* ... */;
// throws if res is truncated
await require('stream').promises.pipeline(res, dst);
```

Don't use `res.pipe(dst)` on its own, which will silently ignore a truncated response.
Since Node.js 13.0, it won't even `.end()` your destination stream, which I assume will make your `dst` stream leak.

If you need to use low-level event handling, you get a response `error` event since Node.js 15.0.

```js
req.on('error', (e) => {
  // failure
});
res.on('error', (e) => {
  // also failure
});
```

I noticed that there's at least one un-deprecated way to detect this that works on all versions I tested.
If you need to support ancient versions of Node.js (high five 🥲), listen for the response `close` event and check if the response `complete` property is false.

```js
res.on('close', () => {
  if (!res.complete) {
    // res is truncated
  }
});
```
