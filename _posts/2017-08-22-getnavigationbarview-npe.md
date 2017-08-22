---
date: Tue Aug 22 2017 00:15:17 -0700 (Pacific Standard Time)
qualitative_time: 
title: NullPointerException in getNavigationBarView
previous_teaser: How to tell if you're eating mango
previous_first: true
---
Problem: your Android Virtual Device boots once, but after that, com.android.systemui repeatedly crashes with

```
FATAL EXCEPTION: main
Process: com.android.systemui, PID: 2833
java.lang.NullPointerException: Attempt to invoke virtual method 'android.view.View android.app.Fragment.getView()' on a null object reference
    at com.android.systemui.statusbar.phone.StatusBar.getNavigationBarView(StatusBar.java:4636)
    at com.android.systemui.statusbar.phone.StatusBarKeyguardViewManager.updateStates(StatusBarKeyguardViewManager.java:537)
    at com.android.systemui.statusbar.phone.StatusBarKeyguardViewManager.reset(StatusBarKeyguardViewManager.java:207)
    at com.android.systemui.statusbar.phone.StatusBarKeyguardViewManager.show(StatusBarKeyguardViewManager.java:145)
    at com.android.systemui.keyguard.KeyguardViewMediator.handleShow(KeyguardViewMediator.java:1721)
    at com.android.systemui.keyguard.KeyguardViewMediator.-wrap15(Unknown Source:0)
    at com.android.systemui.keyguard.KeyguardViewMediator$4.handleMessage(KeyguardViewMediator.java:1482)
    at android.os.Handler.dispatchMessage(Handler.java:105)
    at android.os.Looper.loop(Looper.java:164)
    at android.app.ActivityThread.main(ActivityThread.java:6541)
    at java.lang.reflect.Method.invoke(Native Method)
    at com.android.internal.os.Zygote$MethodAndArgsCaller.run(Zygote.java:240)
    at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:767)
```

Here's [StatusBarKeyguardViewManager.java:537](https://android.googlesource.com/platform/frameworks/base/+/android-8.0.0_r4/packages/SystemUI/src/com/android/systemui/statusbar/phone/StatusBarKeyguardViewManager.java#537).
It's checking if there's a navigation bar view, but [`StatusBar.getNavigationBarView`](https://android.googlesource.com/platform/frameworks/base/+/android-8.0.0_r4/packages/SystemUI/src/com/android/systemui/statusbar/phone/StatusBar.java#4636) would rather crash on null `mNavigationBar` than itself return `null`.

Anyway, check this box and deal with it.

!["Has Hardware Buttons (Back/Home/Menu)" in "Configure Hardware Profile"](http://i.imgur.com/tt0sB1Y.png)
