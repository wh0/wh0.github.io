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

|a[i]   |gzip defaults|bzip2 defaults|xz defaults|brotli defaults|png up    |
|:------|------------:|-------------:|----------:|--------------:|---------:|
|0      |        112  |          47  |      144  |         **14**|     248  |
|i      |     14,293  |       8,642  |    4,376  |        9,963  |   **643**|
|i^2    |     30,847  |      35,016  |   22,636  |       23,065  |**19,827**|
|sqrt(i)|     63,875  |      68,199  | **48,124**|       49,680  |  66,426  |
|sin(i) |     76,016  |      76,503  |   74,580  |     **73,576**|  82,047  |
|i / 7  |     19,086  |      10,670  |    5,332  |        3,378  |   **937**|
|i * 7  |     18,342  |      15,080  |    8,052  |       21,203  |   **935**|
|i * phi|     57,872  |      57,239  |   42,924  |       46,418  | **9,139**|

[View source](https://glitch.com/edit/#!/pushy-tide-pangolin?path=test.sh)

Notes:

- Uncompressed size is 10,000 * 8 = 80,000 bytes.
- sqrt(i) and sin(i) are reminiscent of "[nothing-up-my-sleeve numbers](https://en.wikipedia.org/wiki/Nothing-up-my-sleeve_number#Examples)" used in cryptography.
- i * phi (phi being the golden ratio here) [according to this video that I keep thinking about](https://www.youtube.com/watch?v=sj8Sg8qnjOg) would keep the fractional part from congregating around any particular value
- I didn't have the usual brotli command line tools, so I wrote a little script to pipe data through the library that comes with Node.js.
- png up is `convert -size 8x10000 -depth 8 -define png:compression-filter=2 gray:- png:-` from ImageMagick. Image formats tend to have systems to support gradients, so I was wondering how this would do.
