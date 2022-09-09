---
date: Thu Sep 08 2022 21:06:44 -0700 (Pacific Daylight Time)
qualitative_time: 
title: Ubuntu flavors installer sizes
previous_teaser: A 3D printed jig for reassembling brownie cubes into a full-size brownie
previous_first: true
---
![](/assets/2022/ubuntu-isos-chart.png)

Flavors described as "full" or "complete" have square marks.
Flavors described as "light" have triangle marks.
Flavors described as neither have circle marks.

Notes:

- The installer size is, of course, a limited metric of lightness.
- I've always heard of Lubuntu as the really light one, but it has grown larger than Xubuntu, Ubuntu Budgie, and Ubuntu MATE.
  In 18.10 they [switched to LXQt](https://www.phoronix.com/news/Lubuntu-18.10-LXQt), dunno if that had anything to do with it.
- Ubuntu [switched to GNOME](https://arstechnica.com/gadgets/2017/11/ubuntu-17-10-return-of-the-gnome/) in 17.10, so the Ubuntu GNOME stopped being a flavor.
- Earlier releases mostly didn't go below around 730 MB.
  Was this to avoid wasting space on a CD?
  Wikipedia confirms you can fit 700 MiB (~734 MB) on a CD.
- The Ubuntu GNOME installer sizes were eerily similar to the Xubuntu installer sizes.
  The samples I used for their 16.04 releases were both exactly 1,389,789,184 bytes.
- Canonical Ltd. is a British company, so I suppose they're really flavours.

Methodology details:

- LTS releases only, at the latest point release I could find.
- AMD64 architecture, "desktop" variant in cases where there are variants.
- Data is from Ubuntu releases, Wayback Machine, and other sources.
- Would have been nice to plot GiB (2^30), but Google Sheets make it much easier to do GB (10^9).
