---
date: Sat Jan 04 2020 16:09:52 -0800 (Pacific Standard Time)
qualitative_time: 
title: How Jekyll checks if a link is valid
previous_teaser: Phone manufacturers ranked by charging cable flexibility
previous_first: true
---
Jekyll provides a `link` "tag" in the templating language it uses, where you pass a file that you want to link to, and
when you build the site, it makes sure the file you referred to really exist and
then outputs the URL of the compiled version of that file.

If you'd like to think about how _you_ would implement that, take a moment to do that now.

---

Now here's how Jekyll does it:
[it loops](https://github.com/jekyll/jekyll/blob/v3.8.5/lib/jekyll/tags/link.rb#L21-L25)
over every file in your project until it finds the one you wanted.
