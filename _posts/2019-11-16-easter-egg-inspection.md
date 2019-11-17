---
date: Sat Nov 16 2019 22:52:26 -0800 (Pacific Standard Time)
qualitative_time: 
title: Android Studio's "Code contains easter egg" inspection
previous_teaser: Wire toothbrushes
previous_first: false
---
There's an inspection called "Code contains easter egg" that comes with Android Studio.
This one:

![](/assets/2019/easter-egg-inspection-inspections.png)

How can it systematically detect an easter egg?

It turns out that this inspection is limited to a narrow definition of an easter egg used in the inspection's description.
It finds a certain syntax of Java *code* that tries not to look like code, not features, like a flight simulator hidden in a spreadsheet program.

From [the implementation](https://android.googlesource.com/platform/tools/base/+/2f03004c181baf9d291a9bf992e1b444e83cd82d/lint/libs/lint-checks/src/main/java/com/android/tools/lint/checks/CommentDetector.java#177) of this lint,
we can see that it searches for the string `\u002a\u002f` inside a comment.
That's some kind of escaped form of `*/`.

[Supposedly](https://docs.oracle.com/javase/specs/jls/se7/html/jls-3.html#jls-3.2) it's correct to unescape these sequences before even lexing comments.
But the syntax highlighting in Android Studio doesn't behave this way.

![](/assets/2019/easter-egg-inspection-example.png)

So indeed you might consider that code to be "hidden."

---

Wait, does this mean Unicode string escapes aren't actually parsed in the safe confines of a string literal?

![](/assets/2019/easter-egg-inspection-string.png)

😱️
