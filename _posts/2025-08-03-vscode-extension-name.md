---
date: Sun Aug 03 2025 10:04:25 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Visual Studio Code extension name requirements
previous_teaser: Updating dynamic DNS with addresses from your Gmail "Recent activity" list
previous_first: true
has_highlighting: true
---
[A Microsoft engineer explains](https://github.com/microsoft/vscode-vsce/issues/20),

> > Extension names may only contain `A` through `Z`, `a` through `z`, `0` through `9` and `-`. The extension name must start with an alphabetic or numeric character.
> 
> Publisher names have the same validation rules.

There's no record of where the inner quote came from.

(Clarification: the name is distinct from the "display name," which is shown in most places.)

The Visual Studio Code Extension Manager checks these requirements with [this regular expression](https://github.com/microsoft/vscode-vsce/blob/v3.6.0/src/validation.ts#L4):

```js
const nameRegex = /^[a-z0-9][a-z0-9\-]*$/i;
```

Extra keywords: vscode, extension name allowed characters, publisher name allowed characters, publisher ID allowed characters, format, regex, underscores are not allowed, vsce, extension gallery, Visual Studio Marketplace.
