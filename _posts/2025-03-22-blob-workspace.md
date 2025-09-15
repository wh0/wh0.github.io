---
date: Sat Mar 22 2025 22:12:37 -0700 (Pacific Daylight Time)
qualitative_time: 
title: One student's search history that the world downloaded 1,122,083 times last week
previous_teaser: A computer designed to inspire absolutely zero interest in computers
previous_first: true
has_highlighting: true
---
I myself have several copies across a few computers.
I'll be shipping copies of it to users in a forthcoming project as well.

How? It was a slight packaging mishap, really.

![](/assets/2025/blob-workspace-npm-downloads.png)

```xml
<component name="FindInProjectRecents">
  <findStrings>
    <find>esprima-six</find>
  </findStrings>
</component>
```

_From [https://app.unpkg.com/blob@0.0.5/files/.idea/workspace.xml](https://app.unpkg.com/blob@0.0.5/files/.idea/workspace.xml#L122)._

(Yeah, not web search.
Find-in-project search.
Disappointing content with a sensationalized title?
You'll find that I don't have a comments system (at time of writing).)

This `.idea/workspace.xml` file is where WebStorm (and other JetBrains IDEs) stores various personal state.
GitHub's community consensus is [not to share this file with collaborators](https://github.com/github/gitignore/blob/86d1826a9f3daf4744e7229a26187f81c081744c/Global/JetBrains.gitignore#L5).

This `.idea` directory doesn't appear in the library's version control, and it's only included---accidentally, I presume---in one published version of this package.
But ironically, that's the version that's the most popular.

![](/assets/2025/blob-workspace-npm-versions.png)

_From [https://www.npmjs.com/package/blob/v/0.0.5?activeTab=versions](https://www.npmjs.com/package/blob/v/0.0.5?activeTab=versions)._

I personally came across this from using an old version of `socket.io-client`.
It's a dependency of `engine.io-parser` version 2.2.1.
