---
date: Sun Oct 24 2021 14:29:08 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Command injection through shell-quote
previous_teaser: creative ways to use your extra "unctio"s when you switch from JavaScript to Rust
previous_first: true
---
A couple of weeks ago I found a vulnerability in the `shell-quote` package on npm which would allow command injection in cases where it is indeed used to quote an untrusted input for execution in a shell.
This is now fixed, and the details were disclosed in [CVE-2021-42740](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-42740).

This post only gives a recap of the problem and a recounting of the events surrounding my report.
I'll be presenting a sample exploit and discussing how I came across the vulnerable code in a future post.

# Recap of the problem

The summary of the vulnerability is described in the CVE.
To paraphrase, shell-quote v1.7.2 used [this line](https://github.com/substack/node-shell-quote/blob/v1.7.2/index.js#L13) to do its quoting (for cases where it prefers not to use single quotes or double quotes):

```js
return String(s).replace(/([A-z]:)?([#!"$&'()*,:;<=>?@\[\\\]^`{|}])/g, '$1\\$2');
```

The character class `[A-z]` toward the beginning matches, in addition to uppercase and lowercase letters, the symbols `` [\]^_` ``.
(That's right, I now know [how to typeset a backtick](https://daringfireball.net/projects/markdown/syntax#code) in an inline code span in markdown.)

The replacement for its capturing group `$1` leaves it as is, which inadvertently allows some shell metacharacters to pass through unquoted.

# The fix in v1.7.3

They've corrected the character class to `[A-Za-z]`.

Now the only special functionality from this first capturing group is to avoid escaping colons in some contexts.
I don't know why this package escapes colons in the first place.
Maybe it has some meaning in a shell that I don't use.

# Notes

## Describing this vulnerability

This issue in shell-quote on its own doesn't allow command execution.
It's specifically when an application passes the library's `quote()` output a shell that there can be command injection.
That does seem like the intended use case though.
If the input comes over the network, this can cause some forms of remote code execution.

If the input to `quote()` is trusted, then this won't be a problem as long as the inputs don't contain sequences that trigger the unintended non-letter matches.

The shell-quote library also supports parsing.
If an application only uses that part, then this vulnerability doesn't apply.

## Clerical mishaps

The CVE description has a typo where the opening square bracket is replaced with a curly brace.

I reported this vulnerability through Tidelift, under my GitHub username, @wh0.
The contact indicated that the CVE would credit me as the reporter.
I don't see that it does, but it seems unlikely that anyone would dispute my claim.

# Timeline

**2018/08/13**
shell-quote v1.6.3 releases with the `[A-z]` code.

**2021/10/14**
I discover this vulnerability.
I report it according to the package's repository's SECURITY.md instructions, which says to contact through Tidelift.

**2021/10/15**
A representative at Tidelift acknowledges that they've received the report.

**2021/10/21**
The Tidelift representative notifies me that [a fixed version is released](https://www.npmjs.com/package/shell-quote/v/1.7.3) and [a CVE is assigned](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2021-42740).
The patch, project changelog, and CVE description all disclose the vulnerability.

# Overview of future post

I'm waiting for a downstream vendor to update to the fixed version.

After that, I'll publish a copy of my original report, which includes a walkthrough of a sample exploitation.

I'll also discuss the context of my research on this package.
