---
date: Tue Dec 31 2024 20:57:27 -0800 (Pacific Standard Time)
qualitative_time: 
title: Emoji metrics
previous_teaser: The total amount I've been paid to watch serde_derive compile
previous_first: false
---
Please let there be someone who also wondered about how emojis get typeset.

Okay imagine there's normal text going along on a web page, and then boom, there's an emoji.

## Questions
Let me briefly go through the qualitative basics first.

1. What do we draw for this emoji?
2. Where?
3. How big?
4. How much space around it?

**What?**
The emoji is a character that the text font doesn't have, so we'll draw a glyph from a fallback font that has it, as provided by the browser or operating system or something.
That font will have a glyph for it, thus defining what we should draw for this emoji.
I'll give a list of what emoji fonts some popular platforms use later.

A better discussed topic is how these glyphs are color graphics, in contrast to normal text being a single color that the web page controls.
I'll mention how various emoji fonts do this (yeah there are different ways) but this article isn't really about this.

**Where?**
Once we have the glyph, it's similar to positioning normal text.

You put the emoji glyph to the right of what came before it (assuming this is left-to-right text).
That's the horizontal position.

The surrounding text font has a baseline, and the emoji font also has a baseline.
You line those up.
That's the vertical position.
I'll show where the baseline is on a selection of emoji fonts later.

**How big?**
The surrounding text has a font size.
The emoji font also has a definition of how big its glyphs are relative to the font size.
You make it as big as that font size.
I'll show how large the glyphs are in a selection of emoji fonts later.

**How much space around it?**
The surrounding text font has vertical metrics specifying how much space above and below the baseline it needs.
Likewise, the emoji font also has these vertical metrics.
These are called the "ascent" and "descent" metrics.
Supposedly you also add half of another metric, called the "line gap."
We've lined up the baseline with the surrounding font, so in general the ascent and descent won't match up with the surrounding text.
When the browser lays out the line box you're writing in, it makes the line box tall enough so that both the normal text's and the emoji's ascent and descent fit.
That's the vertical space.

The emoji glyph has a horizontal metric specifying how much space to the right of the previous glyph it needs.
This is called the "advance width."

The ascent and descent from the font and the advance width from the glyph together define a rectangle of space reserved for this glyph.
And typically the glyph will fit inside this rectangle, possibly leaving some space around itself.
I'll show the ascent, descent, and advance width for a selection of emoji fonts and glyphs later.

**Takeaways.**
Emojis come from another font.
They're glyphs, which behave like glyphs that make up normal text.
Typesetting them thus depends on the font metrics and the glyph.

## Platforms and emoji fonts

For quantitative details, we'll have to examine actual emoji fonts.
Here's a list of emoji fonts I examined and the operating systems or browsers where they're used.

**Segoe UI Emoji**, Microsoft Corporation, seen in Windows 10

![](/assets/2024/emoji-metrics-layout-win10.svg)

These previews show some selected emojis alongside text in Arial, at a font size of 100 pixels, or 75 points, with Arial's vertical metrics visualized.

Legend:

![](/assets/2024/emoji-metrics-line-em.svg) 1 em

![](/assets/2024/emoji-metrics-line-ascent-half-leading.svg) Ascent/descent with half-leading

![](/assets/2024/emoji-metrics-line-ascent.svg) Ascent/descent

![](/assets/2024/emoji-metrics-line-cap-height.svg) Cap height

![](/assets/2024/emoji-metrics-line-x-height.svg) x-height

![](/assets/2024/emoji-metrics-line-baseline.svg) Baseline

This version of Microsoft's emoji font defines color glyphs using a part of the font file called the "COLR table."
This approach defines color glyphs as a list of solid colored shapes, like you'd find in a vector graphic.

This font's emoji glyphs have different advance widths for different emoji, which is unique among the fonts that I examine in this article.
The advance width varies from 0.882 em for black/white small square (▪️/▫️) to 1.452 em for Mount Fuji (🗻).

**SamsungColorEmoji**, Google Inc. and Samsung Electronics Co., seen on Samsung phones

![](/assets/2024/emoji-metrics-layout-samsung.svg)

By the way, these aren't real renditions.
They're mockups that I put together from emoji glyphs and my understanding of how they're typeset.
Notably, Samsung phones don't come with anything that's metric compatible with Arial, so you definitely wouldn't see a line like this.

Samsung's emoji font defines color glyphs using a part of the font file called the "CBDT table."
This approach defines color glyphs as raster images, with a few copies provided for a number of sizes.
These raster images are squares that get rendered at a size of 1.174 em side length, although not all of the image area is occupied by opaque pixels.
My analysis of the emoji bounds later ignores transparent pixels.

**Noto Color Emoji**, Google Inc., seen in Android

![](/assets/2024/emoji-metrics-layout-noto.svg)

The copy of Google's emoji font that I examined defines color glyphs using a part of the font file called the "SVG table."
Yeah, the scalable vector graphics SVG.
Indeed, this approach defines color glyphs as SVG images.
I believe Google has a build system that targets other color glyph formats, and they offer copies containing those other formats too.

**Noto Emoji**, Google LLC, which I don't think a major OS uses this but I was curious anyway

![](/assets/2024/emoji-metrics-layout-noto-mono.svg)

This monochrome emoji font, of course, doesn't have color glyphs.

**Twemoji Mozilla**, Mozilla Foundation, Twitter, Inc., and other contributors, seen in Firefox

![](/assets/2024/emoji-metrics-layout-twemoji.svg)

This font also uses the COLR table.

**Segoe UI Emoji**, Microsoft Corporation, seen in Windows 11

![](/assets/2024/emoji-metrics-layout-win11.svg)

This version of Microsoft's emoji font defines color glyphs using the COLR table as well, but with data conforming to a newer format called "version 1."
You often see this referred to as "COLRv1."
This approach defines color glyphs as a list of shapes as before, but with additional support for gradients.

A [Microsoft Design blog post](https://microsoft.design/articles/bringing-new-emoji-to-windows-11/) notes that the font additionally contains flat and monochrome glyphs too, for software that doesn't support COLRv1.
It also informs us, "COLRv1 is paradoxically ethereal, more sensitive, and, like humanity, full of light and inherently social."
Now I'm not someone with a LinkedIn profile that says "I conjure worlds out of words," but it's a vector image format with gradients.

**Apple Color Emoji**, Apple Inc., seen in macOS and iOS

As typeset in macOS:

![](/assets/2024/emoji-metrics-layout-mac.svg)

And as typeset in iOS (the same but slightly lower):

![](/assets/2024/emoji-metrics-layout-ios.svg)

Apple's emoji font defines color glyphs using a part of the font file called the "sbix table."
This approach also defines color glyphs as raster images, with a few copies provided for a number of sizes.

There's actually something to typesetting these other than the metrics and other information in the font.
See how they're partially below the baseline?
The font file itself says to place them entirely above the baseline.
There's something in Apple's proprietary Core Text engine that knows that the emojis ought to be lower.

Some of my own findings:

1. Although there are undocumented parts of the font file, we can tell that the rule to render emoji glyphs below the baseline is not in the font file itself.
   Instructing Core Text to use the font file `/System/Library/Fonts/Apple Color Emoji.ttc`
   (`CFURLCreateWithFileSystemPath`,
   `CGDataProviderCreateWithURL`,
   `CGFontCreateWithDataProvider`,
   `CTFontCreateWithGraphicsFont`)
   results in the engine rendering emoji glyphs as we'd expect from the font metrics, entirely above the baseline.
   Using the font by name
   (`CTFontCreateWithName`)
   results in the engine rendering emoji glyphs partially below the baseline.
2. On macOS, emoji glyphs are shifted down by 0.125 em.
3. On iOS, emoji glyphs are shifted down by 0.15 em.

Allegedly there's a number of other unexplained effects in how macOS and iOS render emoji glyphs.
Luckily, these effects didn't have any effect in the unusually large font sizes I used in my samples.
But you'll probably have extra discrepancies in normal use.

Behdad Esfahbod, who I estimate has 19 years more experience in text rendering than I do, gives the following wisdom regarding this unexplained Apple emoji positioning: "[I can reproduce this and I'm out of ideas.](https://github.com/harfbuzz/harfbuzz/issues/2679#issuecomment-1345663082)"

Another thing though, not positioning related.
When I was trying to get the transparent-pixels-excluded bounds for Apple's emoji, I ran into this issue where some of the emoji images (perhaps the face ones?) have single-pixel dots in the bottom left and top right corners.
Weird.
I'm not including these corner dots in my analysis of the emoji bounds later.

## Comparison

The diagrams below show the bounds of an emoji as rendered on different platforms in their respective emoji fonts.
But first, here are several notes on how to read them:

1. The gray line is the baseline.
2. The rectangle is the bounding box.
   Each platform/font combo is in a different color.
3. The horizontal lines going out from the sides of the rectangle show the advance width.
   If the advance width is not beyond the bounding box, e.g. with Twemoji Mozilla, it just won't be visible.
   The vertical position of these lines doesn't matter; they're spread out so that they don't overlap.
4. The vertical lines going out from the top and bottom of the rectangle show the ascent/descent plus the half-leading.
   The horizontal position of these lines doesn't matter; they're spread out so that they don't overlap.
5. The little horizontal mark on the vertical lines show the ascent/descent without the half-leading.
   Most fonts have zero line gap and thus zero half-leading, where the horizontal mark is drawn at the ends of the vertical lines.
   If the ascent/descent is exactly at the bounding box, e.g. with Twemoji Mozilla, the horizontal mark is drawn over the box and won't be prominent.
6. Your device's native emoji is shown.

For scale, the length of this line represents 1 em:

![](/assets/2024/emoji-metrics-help-scale.svg)

Alright, here are the diagrams.

![](/assets/2024/emoji-metrics-comparison-face.svg)

Legend:

![](/assets/2024/emoji-metrics-box-win10.svg) Segoe UI Emoji from Windows 10

![](/assets/2024/emoji-metrics-box-samsung.svg) SamsungColorEmoji

![](/assets/2024/emoji-metrics-box-noto.svg) Noto Color Emoji

![](/assets/2024/emoji-metrics-box-noto-mono.svg) Noto Emoji

![](/assets/2024/emoji-metrics-box-twemoji.svg) Twemoji Mozilla

![](/assets/2024/emoji-metrics-box-win11.svg) Segoe UI Emoji from Windows 11

![](/assets/2024/emoji-metrics-box-mac.svg) Apple Color Emoji on macOS

![](/assets/2024/emoji-metrics-box-ios.svg) Apple Color Emoji on iOS

More emoji:

![](/assets/2024/emoji-metrics-comparison-body.svg)

In this above one, the Samsung one actually extends below the descent.

In these last two, the Twemoji Mozilla and Apple Color Emoji on macOS bounding boxes are the same, so I made the rectangles kind of dashed between the two colors.

![](/assets/2024/emoji-metrics-comparison-circle.svg)

![](/assets/2024/emoji-metrics-comparison-square.svg)

## Data

Here's the font metrics, in ems.
Positive values refer to above the baseline and negative values refer to below the baseline.

| Font              | Version       | Appears in     | Ascent | Descent | Line gap | x-height | Cap height |
|:------------------|:--------------|:---------------|-------:|--------:|---------:|---------:|-----------:|
| Segoe UI Emoji    | Version 1.29  | Windows 10     |  1.079 |  -0.251 |    0.000 |    0.500 |      0.700 |
| SamsungColorEmoji | Version 2.028 | Samsung phones |  0.928 |  -0.244 |    0.000 |    0.000 |      0.928 |
| Noto Color Emoji  | Version 2.047 | Android        |  0.928 |  -0.244 |    0.000 |    0.500 |      0.700 |
| Noto Emoji        | Version 3.003 | (no major OS)  |  0.928 |  -0.244 |    0.000 |    0.000 |      0.928 |
| Twemoji Mozilla   | Version 0.7.0 | Firefox        |  0.875 |  -0.125 |    0.090 |    0.000 |      0.000 |
| Segoe UI Emoji    | 1.51          | Windows 11     |  1.079 |  -0.251 |    0.000 |    0.500 |      0.700 |
| Apple Color Emoji | 18.3d3e1      | macOS, iOS     |  1.000 |  -0.313 |    0.000 |    0.625 |      1.000 |
| Arial             | Version 7.00  | Windows 10     |  0.905 |  -0.212 |    0.033 |    0.519 |      0.716 |

Arial is here too, because we were juxtaposing emojis with it.
I have read somewhere that Arial is metrics-compatible with Helvetica, so you elitists just imagine that's there too.

If you're the type of person who's aware that these font files have multiple sets of ascent and descent metrics and that the situation is out of control, I am confirming that the situation is still out of control.
I had to consult different ones for different entries in this table based on what each platform uses.

And here's the glyph metrics for select emojis.

| Font                        | Glyph | Advance | X min |  Y min | X max | Y max | Width | Height |
|-----------------------------|-------|--------:|------:|-------:|------:|------:|------:|-------:|
| Segoe UI Emoji (Windows 10) | 😃     |   1.373 | 0.125 | -0.210 | 1.248 | 0.913 | 1.123 |  1.123 |
| Segoe UI Emoji (Windows 10) | 🧍     |   0.882 | 0.125 | -0.210 | 0.757 | 0.913 | 0.632 |  1.123 |
| Segoe UI Emoji (Windows 10) | ⚫     |   1.373 | 0.125 | -0.210 | 1.248 | 0.913 | 1.123 |  1.123 |
| Segoe UI Emoji (Windows 10) | ⬛     |   1.373 | 0.125 | -0.210 | 1.248 | 0.913 | 1.123 |  1.123 |
| SamsungColorEmoji           | 😃     |   1.172 | 0.043 | -0.207 | 1.130 | 0.880 | 1.087 |  1.087 |
| SamsungColorEmoji           | 🧍     |   1.172 | 0.402 | -0.250 | 0.783 | 0.913 | 0.380 |  1.163 |
| SamsungColorEmoji           | ⚫     |   1.172 | 0.022 | -0.228 | 1.152 | 0.902 | 1.130 |  1.130 |
| SamsungColorEmoji           | ⬛     |   1.172 | 0.043 | -0.207 | 1.130 | 0.880 | 1.087 |  1.087 |
| Noto Color Emoji            | 😃     |   1.245 | 0.088 | -0.160 | 1.150 | 0.863 | 1.063 |  1.023 |
| Noto Color Emoji            | 🧍     |   1.245 | 0.386 | -0.207 | 0.860 | 0.889 | 0.473 |  1.096 |
| Noto Color Emoji            | ⚫     |   1.245 | 0.073 | -0.207 | 1.172 | 0.892 | 1.099 |  1.099 |
| Noto Color Emoji            | ⬛     |   1.245 | 0.073 | -0.207 | 1.171 | 0.891 | 1.098 |  1.098 |
| Noto Emoji                  | 😃     |   1.270 | 0.127 | -0.164 | 1.143 | 0.847 | 1.016 |  1.011 |
| Noto Emoji                  | 🧍     |   1.270 | 0.347 | -0.166 | 0.923 | 0.850 | 0.576 |  1.016 |
| Noto Emoji                  | ⚫     |   1.270 | 0.127 | -0.166 | 1.143 | 0.850 | 1.016 |  1.016 |
| Noto Emoji                  | ⬛     |   1.270 | 0.127 | -0.166 | 1.143 | 0.850 | 1.016 |  1.016 |
| Twemoji Mozilla             | 😃     |   1.000 | 0.000 | -0.125 | 1.000 | 0.875 | 1.000 |  1.000 |
| Twemoji Mozilla             | 🧍     |   1.000 | 0.327 | -0.123 | 0.673 | 0.855 | 0.346 |  0.979 |
| Twemoji Mozilla             | ⚫     |   1.000 | 0.000 | -0.125 | 1.000 | 0.875 | 1.000 |  1.000 |
| Twemoji Mozilla             | ⬛     |   1.000 | 0.000 | -0.125 | 1.000 | 0.875 | 1.000 |  1.000 |
| Segoe UI Emoji (Windows 11) | 😃     |   1.373 | 0.194 | -0.140 | 1.179 | 0.845 | 0.985 |  0.985 |
| Segoe UI Emoji (Windows 11) | 🧍     |   1.373 | 0.525 | -0.143 | 0.848 | 0.848 | 0.323 |  0.990 |
| Segoe UI Emoji (Windows 11) | ⚫     |   1.373 | 0.178 | -0.140 | 1.164 | 0.845 | 0.985 |  0.985 |
| Segoe UI Emoji (Windows 11) | ⬛     |   1.373 | 0.194 | -0.140 | 1.179 | 0.845 | 0.985 |  0.985 |
| Apple Color Emoji (macOS)   | 😃     |   1.000 | 0.044 | -0.088 | 0.963 | 0.831 | 0.919 |  0.919 |
| Apple Color Emoji (macOS)   | 🧍     |   1.000 | 0.331 | -0.119 | 0.669 | 0.856 | 0.338 |  0.975 |
| Apple Color Emoji (macOS)   | ⚫     |   1.000 | 0.000 | -0.125 | 1.000 | 0.875 | 1.000 |  1.000 |
| Apple Color Emoji (macOS)   | ⬛     |   1.000 | 0.000 | -0.125 | 1.000 | 0.875 | 1.000 |  1.000 |
| Apple Color Emoji (iOS)     | 😃     |   1.000 | 0.044 | -0.113 | 0.963 | 0.806 | 0.919 |  0.919 |
| Apple Color Emoji (iOS)     | 🧍     |   1.000 | 0.331 | -0.144 | 0.669 | 0.831 | 0.338 |  0.975 |
| Apple Color Emoji (iOS)     | ⚫     |   1.000 | 0.000 | -0.150 | 1.000 | 0.850 | 1.000 |  1.000 |
| Apple Color Emoji (iOS)     | ⬛     |   1.000 | 0.000 | -0.150 | 1.000 | 0.850 | 1.000 |  1.000 |

---

Alright, that's it.
Any more fussing with this article and I'll have to rename a bunch of paths because I'm including... 😬 27 images, all going into an assets directory that's organized by year.
