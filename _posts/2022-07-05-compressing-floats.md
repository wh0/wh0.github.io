---
date: Tue Jul 05 2022 16:11:08 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Compressing floats
previous_teaser: The fear of not being able to program your own Markdown parser while doing all your writing in Markdown
previous_first: true
---
Okay so if you had a bunch of packed binary floats, would they compress well?
If they would repeat, then sure.
What about the ~~natural numbers~~ nonnegative integers lmao 0, 1, 2, ...?
Or simple sequences?

Here are my results on 10,000 64-bit floats.
Various sequences, various compression tools.

![](/assets/2022/compressing-floats-chart.png)

|a[i]|gzip defaults|bzip2 defaults|xz defaults|brotli defaults|png up|
|:----|----:|----:|----:|----:|----:|
|0|112|47|144|14|248|
|i|14293|8642|4376|9963|643|
|i^2|30847|35016|22636|23065|19827|
|sqrt(i)|63875|68199|48124|49680|66426|
|sin(i)|76016|76503|74580|73576|82047|
|i / 7|19086|10670|5332|3378|937|
|i * 7|18342|15080|8052|21203|935|
|i * phi|57872|57239|42924|46418|9139|

[View source](https://glitch.com/edit/#!/pushy-tide-pangolin?path=test.sh)

Notes:

- Uncompressed size is 10,000 * 8 = 80,000 bytes.
- sqrt(i) and sin(i) are reminiscent of "[nothing-up-my-sleeve numbers](https://en.wikipedia.org/wiki/Nothing-up-my-sleeve_number#Examples)" used in cryptography.
- i * phi (phi being the golden ratio here) [according to this video that I keep thinking about](https://www.youtube.com/watch?v=sj8Sg8qnjOg) would keep the fractional part from congregating around any particular value
- I didn't have the usual brotli command line tools, so I wrote a little script to pipe data through the library that comes with Node.js.
- png up is `convert -size 8x10000 -depth 8 -define png:compression-filter=2 gray:- png:-` from ImageMagick. Image formats tend to have systems to support gradients, so I was wondering how this would do.
