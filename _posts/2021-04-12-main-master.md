---
date: Mon Apr 12 2021 23:05:45 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Default branches at work
previous_teaser: How to introduce a character that won't turn out to be a super villain
previous_first: true
---
```
$ history | tee history.txt | wc -l
1000
$ grep '  gi' history.txt | wc -l
551
$ grep '  gi' history.txt | grep main | wc -l
29
$ grep '  gi' history.txt | grep master | wc -l
31
```

![Default branch name representation in shell history. main: 29, master: 31, other git: 491, non-git: 449](/assets/2021/main-master-chart.svg)

(I manually verified that no command mentioned both.)

At work, I'm now working in some Git repositories that use `main` as the name of the default branch.
The team I'm in is using GitHub, which had earlier [switched over their default for new repositories](https://github.blog/changelog/2020-10-01-the-default-branch-for-newly-created-repositories-is-now-main/).

I do most of my Git stuff on the command line, so here's a look at the adoption, quantized by statistical data from a snapshot of my shell history taken today.
I'll call out the following:

- The history is pretty short.
  Even my terminal scrollback is longer.
- The representation is just about balanced.
  If the primary annoyance is pulling up the wrong variant from a [Ctrl+R search](https://www.gnu.org/software/bash/manual/html_node/Commands-For-History.html), I'm experiencing the peak of that now.
- Apparently I don't do much else on the command line.
