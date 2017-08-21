---
date: Mon Aug 21 2017 11:10:44 -0700 (PDT)
qualitative_time: 
title: Time zones in Jekyll on Ruby 2.4
previous_teaser: Sweeping out the inside of a dustpan
previous_first: false
---
GitHub Pages upgraded the Ruby interpreter they use to run Jekyll.
With that, it started to use the time zone specified by the `date` variable in each post's front matter instead of the global `timezone` variable from the site's `_config.yml` file.
Prior to GitHub pages making the switch, this change in behavior was [reported](https://github.com/jekyll/jekyll/issues/5963) as a bug in Jekyll.

(I have a screenshot for this, but it's on another computer.)

This change, combined with several other factors, led to most of post permalinks breaking for a few days.
It happened to be the case that:

- I live far west, with a big "minus" time zone.
- I wrote most of the posts in the evening.
- The [script](https://github.com/wh0/wh0.github.io/blob/967d52d3a6518ccc1a7c236548d4bdd28adae6f2/index.html#L7) I used to generate the `date` variable in the front matter always used UTC for simplicity.
- Jekyll (by default?) files post pages into directories for the year, month, and day.

And then all the days in the URL paths changed.
What a thing to have happened.
