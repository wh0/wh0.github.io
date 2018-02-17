---
date: Sat Feb 17 2018 11:52:59 -0800 (PST)
qualitative_time: 
title: build_file_checksums.ser
previous_teaser: Losing weight in 14 minutes a day with PostgreSQL
previous_first: true
has_highlighting: true
---
The preview versions of Android Studio (Beta and Canary channels) are closed source software.
The license of the upstream project, IntelliJ IDEA Community Edition, permits this distribution of derivative works without source code.

Version 3.2 of Android Studio (currently the Canary release) creates a file called `.idea/caches/build_file_checksums.ser` in your project. A few things are immediately apparent:

* It's not included in the default `.gitignore`.
* It's in some binary format that also contains readable strings.
* It changes sometimes.
* Different instances are different sizes.

There's no documentation about this file.
The current Canary release is like a black box that puts a rock in your shoe.
That bugs me.

Anyway, I did some investigation.

* The `file` utility says it's *Java serialization data, version 5*.
* A string in the file mentions `com.android.tools.idea.gradle.project.ProjectBuildFileChecksums`

Sources for the stable version of the Android Gradle plugin are available, but the `ProjectBuildFileChecksums` class isn't present (which is consistent with the stable version not creating this file).

Java serialization data has a lot of metadata.
I built this dummy class and tried deserializing the file from an Android app that I'm working on.

```java
package com.android.tools.idea.gradle.project;

import java.io.Serializable;
import java.util.Map;

public class ProjectBuildFileChecksums implements Serializable {

	private static final long serialVersionUID = 4924462801735608105L;

	long myLastGradleSyncTimestamp;
	Map myFileChecksums;

}
```

The `myFileChecksums` map contains (`String`, `byte[16]`) mappings, for the following keys:

* `settings.gradle`
* (your absolute home directory)`/.gradle/gradle.properties`
* `build.gradle`
* `local.properties`
* `gradle.properties`
* `app/build.gradle`

That second entry, with your absolute home directory, is probably what introduces the varying size, with different people using different operating systems and with different usernames.
