---
date: Wed Oct 19 2022 17:48:31 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Firefox network monitor columns
previous_teaser: My public apology for liking chunky peanut butter
previous_first: false
---
Okay, you know the network monitor in the browser developer tools?
This one:

![](/assets/2022/devtools-columns-0.png)

You can open this details panel:

![](/assets/2022/devtools-columns-open.png)

And close it again:

![](/assets/2022/devtools-columns-1.png)

That makes timeline column get smaller.
Doing it again makes it even smaller:

![](/assets/2022/devtools-columns-chart.png)

(Data from Firefox 105 (I did the measurements a few days ago) on Windows, maximized 1920x1080, developer tools docked to bottom.)

You can actually make it smaller than it lets you by dragging the divider (shown in red in the chart).

And if you're wondering why I wrote this up and put together all the graphics instead of being a good person and reporting this in their bug tracker, (1) I resent the imagined use of "instead" as if they're mutually exclusive, and (2) someone else [already reported it](https://bugzilla.mozilla.org/show_bug.cgi?id=1782167).

Also here's one more graphic I made:

![](/assets/2022/devtools-columns-stacked.png)
