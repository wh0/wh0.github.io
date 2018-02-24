---
date: Sat Feb 24 2018 00:55:48 -0800 (Pacific Standard Time)
qualitative_time: 
title: Firebase Messaging License
previous_teaser: Best haircuts for Wi-Fi reception
previous_first: true
---
I've been working on an Android app.
It uses Firebase Cloud Messaging.
Lately, I've been thinking about publishing it.
Can I though?

---

According to firebase-messaging-11.8.0.aar, in AndroidManifest.xml,

```xml
     Licensed under the Apache License, Version 2.0 (the "License");
```

but **probably not**, unless you somehow construe "this file" to mean the entire archive.
That would be a stretch.

---

According to firebase-messaging-11.8.0.pom,

```xml
<licenses><license><name>Android Software Development Kit License</name><url>https://developer.android.com/studio/terms.html</url><distribution>repo</distribution></license></licenses>
```

and according to that license,

> 3.4 ... you may not copy (except for backup purposes), modify, adapt, redistribute, decompile, reverse engineer, disassemble, or create derivative works of the SDK or any part of the SDK.

so **no**.

---

According to [a page that claims to be Firebase's Terms of Service](https://www.firebase.com/terms/terms-of-service.html),

> ... Company hereby grants You, solely during the term of this Agreement, a non-exclusive, non-transferable, revocable license, without rights to sublicense, to incorporate libraries and APIs that are included in the Company Software into Your Applications, to reproduce and distribute such libraries and APIs together with Your Applications ...

but **probably not**, but there's no indication that the Firebase Messaging library for Android has anything to do with that agreement.
