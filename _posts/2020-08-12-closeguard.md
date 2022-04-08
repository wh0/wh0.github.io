---
date: Wed Aug 12 2020 17:12:17 -0700 (Pacific Daylight Time)
qualitative_time: 
title: "\"A resource failed to call close.\""
previous_teaser: An electric fan's opinion on whether I should go to bed earlier
previous_first: false
has_highlighting: true
---

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);

	// Add this:
	StrictMode.setVmPolicy(new StrictMode.VmPolicy.Builder(StrictMode.getVmPolicy())
			.detectLeakedClosableObjects()
			.build());

	// Other stuff, as before.
}
```

And then it'll log out **where that un-closed thing was constructed**, and you can look there and add some code to close it.

---

Folks, I've been informed that, prior to this addition, people had to spend on average 8 minutes and 20 seconds on this page.

A lot of these visitors arrive from searching for the error message.
I'm just gonna put the "right answer" up there, and they can get on with their day.

Of course, you're welcome to read the original post below, which is more of a discussion on why this error is particularly resistant to typical debugging techniques.

---

Some errors you'll never find by searching for them in the platform's source code, like this one (from the Android application framework):

```
W/System: A resource failed to call close.
```

This one is like that because the word "close" is [added programmatically](https://cs.android.com/android/platform/superproject/+/android-10.0.0_r30:libcore/dalvik/src/main/java/dalvik/system/CloseGuard.java;l=279):

```java
System.logW("A resource failed to call "
        + (String) closerNameOrAllocationInfo + ". ");
```

So for different kinds of resources, you'll see messages like `A resource failed to call destroy.` or `A resource failed to call release.` or `A resource failed to call end.`. (Sorry, you knew what the code meant. Just including these SEO reasons.)

I'm glad I tracked this down.
I found out from the code around there that you can have it retain a stack trace of where this never-closed resource was created.

```java
/**
 * True if CloseGuard stack capture and tracking are enabled.
 */
public static boolean isEnabled() {
    return stackAndTrackingEnabled;
}
```

This is in a class `dalvik.system.CloseGuard` that's hidden from the public API, ~~so I guess one would access it through reflection.
So uh, probably only do this in debugging.~~
**Update**: [pallgeuer points out](https://stackoverflow.com/a/64929520/1864688) that it's available in the [StrictMode](https://developer.android.com/reference/android/os/StrictMode.VmPolicy.Builder#detectLeakedClosableObjects()) API.
But to the theme of this post, some callsites you'll never find using the platform's cross reference tool, like this one.
Anyway, here's the old reflection code I had posted, but you might as well use the public API instead.

```java
try {
	Class.forName("dalvik.system.CloseGuard")
			.getMethod("setEnabled", boolean.class)
			.invoke(null, true);
} catch (ReflectiveOperationException e) {
	throw new RuntimeException(e);
}
```

Then you get a nice stack trace like this:

```
D/StrictMode: StrictMode policy violation: android.os.strictmode.LeakedClosableViolation: A resource was acquired at attached stack trace but never released. See java.io.Closeable for information on avoiding resource leaks.
        at android.os.StrictMode$AndroidCloseGuardReporter.report(StrictMode.java:1877)
        at dalvik.system.CloseGuard.warnIfOpen(CloseGuard.java:286)
        at sun.nio.fs.UnixDirectoryStream.finalize(UnixDirectoryStream.java:240)
        at java.lang.Daemons$FinalizerDaemon.doFinalize(Daemons.java:289)
        at java.lang.Daemons$FinalizerDaemon.runInternal(Daemons.java:276)
        at java.lang.Daemons$Daemon.run(Daemons.java:137)
        at java.lang.Thread.run(Thread.java:919)
     Caused by: java.lang.Throwable: Explicit termination method 'close' not called
        at dalvik.system.CloseGuard.open(CloseGuard.java:237)
        at sun.nio.fs.UnixDirectoryStream.<init>(UnixDirectoryStream.java:76)
        at sun.nio.fs.UnixSecureDirectoryStream.<init>(UnixSecureDirectoryStream.java:63)
        at sun.nio.fs.UnixFileSystemProvider.newDirectoryStream(UnixFileSystemProvider.java:436)
        at java.nio.file.Files.newDirectoryStream(Files.java:457)
        (... a bunch of project-specific frames ...)
        at android.os.AsyncTask$SerialExecutor$1.run(AsyncTask.java:289)
        at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1167)
        at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:641)
        at java.lang.Thread.run(Thread.java:919) 
```
