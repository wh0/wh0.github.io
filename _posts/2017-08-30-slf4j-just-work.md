---
date: Wed Aug 30 2017 22:46:28 -0700 (Pacific Standard Time)
qualitative_time: 
title: How to get SLF4J to just work
previous_teaser: The smell of the inside of your nostrils
previous_first: true
---
Get [slf4j-simple](https://bintray.com/bintray/jcenter/org.slf4j%3Aslf4j-simple).
Create a `simplelogger.properties` file in your classpath with the content:

    org.slf4j.simpleLogger.defaultLogLevel=trace

If you're building an Android app, you would put that file in your app module's `src/main/resources` directory.
Now you can see logs in stderr.

But that's just my idea of working.

---

Sometimes it feels like the world is working against you, and you run into this evil thing that has made it its mission to swallow any debugging information from a library you're using.
And you just want to see what's going wrong, but now you're stuck contemplating what even is a "logging facade," let alone a simple one.
And an hour passes by, and you're still staring at a message that says,

    SLF4J: Defaulting to no-operation (NOP) logger implementation

Some days are like that.
