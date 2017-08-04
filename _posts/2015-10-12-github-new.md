---
date: Sun Oct 11 2015 23:58:13 -0700 (PDT)
title: GitHub's online editing tools
---
You can create new files on the web in GitHub.
According to [some blog post](https://github.com/blog/1327-creating-files-on-github),
you can also create a URL that prepopulates the file name.

There are a couple of issues with this when the file name is under a directory:

* Clicking the "cancel" link on the page takes you to a 404 page.
* The directory isn't split off into a breadcrumb entry.
  It becomes so when you try to edit the file name.
  And it messes up the cursor position when it does it.

I find that `value`, `message`, and `description` parameters also work.
This is not intended as an act of reverse engineering.
