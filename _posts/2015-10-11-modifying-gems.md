---
date: 2015-10-10 23:00:00 -0700
title: Modifying other people's Ruby gems
has_highlighting: true
---
It's pretty easy to modify other people's gems.
They have have these things called "gemspecs," which let you build a file that you can install with the `gem` command:

{% highlight sh %}
git clone https://github.com/jekyll/jekyll-coffeescript.git
cd jekyll-coffeescript
# make some changes
gem build jekyll-coffeescript.gemspec
gem install jekyll-coffeescript-1.0.1.gem
{% endhighlight %}

I know this because I spent some time trying to make Jekyll 2 not need a JavaScript runtime.
And I did that because I was gonna install Jekyll to make this site.

I thought I could get away without a JavaScript runtime by getting the pre-release version 3.0.0.pre.beta9.
But it turns out that version [doesn't do dependencies right](https://github.com/jekyll/jekyll/issues/3411).

Anyway, go support [my pull request](https://github.com/jekyll/jekyll-coffeescript/pull/11) for great dependency disentanglement.
