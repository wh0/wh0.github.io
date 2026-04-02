---
date: Wed Apr 01 2026 21:36:43 -0700 (Pacific Daylight Time)
qualitative_time: 
title: How far down you can go with nested subscript elements on GitHub
previous_teaser: A keyboard designed for supertasters
previous_first: false
has_highlighting: true
---
Let's walk through what happens if you use a bunch of nested `<sub>` subscript elements on a GitHub comment or whatever.

They have this CSS:

```css
sub {
  /* this undoes the browser's vertical-align: sub */
  vertical-align: baseline;
  font-size: 75%;
  /* don't know what this is for */
  line-height: 0;
  position: relative;
  bottom: -.25em;
}
```

So each nested `<sub>` makes the text smaller and moves it down an amount relative to its new size.

You'd recognize this as a geometric series, which converges to a total of 0.75em downward, which is 10.5px at the 14px comment text size.

Here's this following snippet rendered:

```markdown
H0<sub>H1<sub>H2<sub>H3<sub>H4<sub>H5<sub>H6</sub></sub></sub></sub></sub></sub>
```

![](/assets/2026/github-sub-sub.png)

For an undocumented reason, three and deeper nestings of `<sub>` are discarded.

So you can only go down 0.328125em, which is 4.59375px at the 14px comment text size.
