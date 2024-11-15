---
date: Thu Nov 14 2024 22:47:43 -0800 (Pacific Standard Time)
qualitative_time: 
title: Android itself surviving Android 15 edge-to-edge enforcement (for View dinosaurs)
previous_teaser: How deaf people tell chocolate chip cookies from oatmeal raisin
previous_first: false
has_highlighting: true
---
"Apps are edge-to-edge by default on devices running Android 15 if the app is targeting Android 15 (API level 35)," [they announce](https://developer.android.com/about/versions/15/behavior-changes-15#edge-to-edge).
And they show you some neat animations of an app with content scrolling behind the little line navigation bar.
So they've made your UI fancier for you?
Nice.

In the next paragraph, they hit you with the more sinister truth.
"This is a breaking change that might negatively impact your app's UI."
All they've done is break your UI and hope that now that you have to go fix it, you'll be inspired to make it fancier, yourself.

![](/assets/2024/edgetoedge-diagram.svg)

_Illustration of activity window layout
before edge-to-edge enforecment,
under edge-to-edge enforcement, and
using various fixes.
Gaps are added to clarify what's behind what._

I don't approve of that tactic, so I'm going to show you how to fix this without actually making your UI fancier.
We'll be looking at examples of what the Android team themselves are doing in order to survive this.

## Don't bump targetSdk

Example:
[ActivitySceneTransitionBasic Sample](https://github.com/android/animation-samples/blob/b9ab54ca88d3730da9ab05d571bfdd0c80dcbcb7/ActivitySceneTransitionBasic/app/build.gradle#L25)

(Side note:
Did I intentionally dig up an old, obscure codebase to make this point?
Not especially.
This is the first sample in their ["UI (Views)" category](https://developer.android.com/samples?category=androiduserinterfaceviews).)

You gotta cover your ears and tell yourself the highest possible number is not the right one.

Using `targetSdk 34` doesn't apply edge-to-edge enforcement, and Google says [it's still allowed on Google Play](https://support.google.com/googleplay/android-developer/answer/11926878?hl=en) until probably 2025 August.

## Set windowOptOutEdgeToEdgeEnforcement

Reference link:
[windowOptOutEdgeToEdgeEnforcement](https://developer.android.com/reference/android/R.attr#windowOptOutEdgeToEdgeEnforcement)

Example:
[Blocked numbers screen](https://android.googlesource.com/platform/packages/services/Telecomm/+/7ba4468d5333be22e374b9e8fd30d2caac45f3ed%5E%21/#F1)

So yeah, there's this escape hatch, in the form of... an integer constant?
Here's how you use this attribute.

In your AndroidManifest.xml:

```xml
<manifest ... >
    <application ... >
        <!-- if you don't have a theme set, add one -->
        <activity
             android:theme="@style/Theme.Telecom.BlockedNumbers"
             ... >
```

In your res/values/styles.xml:

```diff
 <resources>
     <!-- in their app there's a little inheritance chain, but feel free to do
          yours directly -->
     <style name="Theme.Telecom.DialerSettings" parent="@android:style/Theme.DeviceDefault.Light">
+        <item name="android:windowOptOutEdgeToEdgeEnforcement">true</item>
     </style>
     <style name="Theme.Telecom.BlockedNumbers" parent="Theme.Telecom.DialerSettings">
```

It's meant to be temporary though.
From the documentation on this attribute:

> However, this attribute will be deprecated and disabled in a future SDK level.

We can keep an eye on these built-in apps to see what happens to them though.
Several places where they used this have already been migrated to the next technique.

## Set fitsSystemWindows

Reference link:
[android:fitsSystemWindows](https://developer.android.com/reference/android/view/View#attr_android:fitsSystemWindows)

Example:
[Captive Portal Login app](https://android.googlesource.com/platform/packages/modules/CaptivePortalLogin/+/b6980f756ec734d3e15d56cf86a39aafdb77f587), a special app for displaying the page where you sign in to the public Wi-Fi at a store or whatever

You find a View that goes outside the stuff that ought not be overlapped by the status bar, app bar, navigation bar, etc.
Put this attribute on it.
Then the framework will set the padding of this View to push that stuff out from under those bars.

In your res/layout/activity_captive_portal_login.xml:

```diff
 <FrameLayout
+    android:fitsSystemWindows="true"
     ... >
```

## Register a View.OnApplyWindowInsetsListener to forward insets to padding

Reference link:
[View.setOnApplyWindowInsetsListener](https://developer.android.com/reference/android/view/View#setOnApplyWindowInsetsListener(android.view.View.OnApplyWindowInsetsListener))
and its dystopian we-shall-do-everything-through-a-compatibility-shim twin
[ViewCompat.setOnApplyWindowInsetsListener](https://developer.android.com/reference/androidx/core/view/ViewCompat#setOnApplyWindowInsetsListener(android.view.View,androidx.core.view.OnApplyWindowInsetsListener))

Example: [Settings app](https://android.googlesource.com/platform/packages/apps/Settings/+/refs/tags/android-15.0.0_r5/src/com/android/settings/homepage/SettingsHomepageActivity.java#395)

You install a little extra program into a View that says to add padding.
This is a horrific imperative COMEFROM-style thing, but it's the approach that leads into actual effort toward making your UI fancier, and it's the approach that the Android team recommends.

In your src/com/android/settings/homepage/SettingsHomepageActivity.java:

```diff
     @Override
     protected void onCreate(Bundle savedInstanceState) {
         super.onCreate(savedInstanceState);
+        setupEdgeToEdge();
     }

+    private void setupEdgeToEdge() {
+        WindowCompat.setDecorFitsSystemWindows(getWindow(), false);
+        ViewCompat.setOnApplyWindowInsetsListener(findViewById(android.R.id.content),
+                (v, windowInsets) -> {
+                    Insets insets = windowInsets.getInsets(WindowInsetsCompat.Type.systemBars());
+                    // Apply the insets paddings to the view.
+                    v.setPadding(insets.left, insets.top, insets.right, insets.bottom);
+
+                    // Return CONSUMED if you don't want the window insets to keep being
+                    // passed down to descendant views.
+                    return WindowInsetsCompat.CONSUMED;
+                });
+    }
```

Note that this listener is set on the view with ID `android.R.id.content`.
That's the great big container (yellow in the diagram at the top of this post) set up by the window decor that goes under the app bar where all your stuff goes.
When you do `Activity.setContentView`, it adds your layout as a child of this "content" container.

This listener replicates the behavior of [the stock onApplyWindowInsets implementation](https://android.googlesource.com/platform/frameworks/base/+/refs/tags/android-15.0.0_r5/core/java/android/view/View.java#12551) when its `fitsSystemWindows` is true, which is to set the view's padding equal to the "system bars" insets.

You also get [similar code](https://android.googlesource.com/platform/tools/base/+/refs/tags/studio-2024.2.1-patch02/wizard/template-impl/src/com/android/tools/idea/wizard/template/impl/activities/emptyActivity/src/emptyActivityKt.kt#40) when you go in Android Studio and create a new activity, except that it sets the listener on a View from inside your layout, rather than the "content" container from the window decor.

---

Extra note, we should probably keep an eye on the Compatibility Test Suite (CTS) tests.
I learned about several of these workarounds looking through recent edge-to-edge related changes in that project, although I later switched out the examples above with occurrences in other apps that I think people have a good chance of seeing on their own phone.

See these [tests for basic Views like FrameLayout](https://android.googlesource.com/platform/cts/+/refs/tags/android-15.0.0_r5/tests/tests/widget/res/layout/framelayout_layout.xml)?
They haven't had to change for [ten years](https://android.googlesource.com/platform/cts/+log/refs/tags/android-15.0.0_r5/tests/tests/widget/res/layout/framelayout_layout.xml) (logs split due to file move, [continues](https://android.googlesource.com/platform/cts/+log/4ca04c0729a0e810b5af16edcd52bb8cb59bf55e/tests/res/layout/framelayout_layout.xml)).
That's how momentous this breakage is.
That's how plainly Android 15 itself is not Android compatible, they had to "fix" the test suite.

## Misc Q & A

- **The behavior changes doc only says "Apps are edge-to-edge by default."
  Isn't that to say there's an option, merely which the default is changing?**
  I think this is a clerical error on their part.
  It is largely described as _enforcement_.
- **The Window class has its own setDecorFitsSystemWindows method, wouldn't that be simpler than finding a suitable view to set fitsSystemWindows on?**
  As part of edge-to-edge _enforcement_, they have removed the functionality of this method.
  There's literally an `if (mEdgeToEdgeEnforced) { return; }` [at the top](https://android.googlesource.com/platform/frameworks/base/+/refs/tags/android-15.0.0_r5/core/java/com/android/internal/policy/PhoneWindow.java#4109).
- **Why go through the complexity of setOnApplyWindowInsetsListener on android.R.id.content when setFitsSystemWindows does the same thing, i.e. with padding?**
  The `content` view and other views that belong to the "decor" have a special private flag that prevents them from doing anything in edge-to-edge mode.
