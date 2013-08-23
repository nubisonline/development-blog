---
layout: post
title: "jekyll-mention-schema: Plugin that allows you to mention a co-author with Schema.org notation"
categories: jekyll-plugin
tags: [meta]
description: jekyll-mention-schema is a plugin for the Jekyll blogging system that alllows you to mention a co-author using Schema.org notation
author: tim
---

I'm honored to bring you the first meta blog post.
Since we'll we sharing the source code of this blog with the world we thought it would be a nice idea to also write about said code here.
Most features will be implemented as Jekyll plugins so you can easily submodule them in your own blogs.
Today, I announce the first of those plugins.

[jekyll-mention-schema](http://github.com/nubisonline/jekyll-mention-schema) is a very simple way to mention your co-authors.
It will do this using [Schema.org](http://schema.org) notation which means the output is easily parsed by machines (such as your favorite search engine).

The plugin depends on an array of authors in your global site config like this:

{% highlight yaml %}
authors:
  user1:
    display_name: 1User
    full_name: User von Wanningston
    gplus_id: 118082699456363103031
    position: Senior VP of Jekyll plugins
  otherauth:
    display_name: Otter
    full_name: Oth&eacute;r Auteur
    gplus_id: 118082699456363103031
    position: Blagosphere Watcher
{% endhighlight %}

Then, you can mention a user like this: `{% raw %}{% mention user1 %}{% endraw %}`, which will result in the following HTML output.

{% highlight html %}
<span itemscope itemtype='http://schema.org/Person'>
	<meta itemprop='name' content='User von Wanningston' />
	<meta itemprop='jobTitle' content='Senior VP of Jekyll plugins' />
	<a href='https://plus.google.com/118082699456363103031' itemprop='url'>1User</a>
</span>
{% endhighlight %}

And so ends the very first meta post. The next one will be by {% mention robbert %} (oh yes, I just totally mentioned Robbert using jekyll-mention-schema. Don't believe me? Check the [source]({{ site.source_link }}{{ page.path }}) of this page).
