---
layout: post
title: "jekyll-xkcd-embed: Plugin for easily embedding xkcd comics into your blog posts"
categories: jekyll-plugin
tags: [meta]
description: jekyll-xkcd-embed is a plugin for the Jekyll blogging system that alllows you to embed a xkcd comic
author: robbert
---

In everyday conversation, there's often a relevant-xkcd-moment. When the conversation has reached a state which has been described in a xkcd comic. In writing, these moments occur too. This is why {% mention tim %} decided that we needed an easy way to embed xkcd comics in our blog posts.

[jekyll-xkcd-embed](https://github.com/nubisonline/jekyll-xckd-embed) is a plugin that allows you to embed a xkcd comic very easily. You can embed a comic by using: `{% raw %}{% xkcd 6 %}{% endraw %}`. This will then result in either a link if no comic was found, or the comic in the following format: {% xkcd 688 %}

You are now ready to use the plugin, but you probably want to do some styling of the image so let's look at the returned html. Say we embed a non-existing xkcd, for example comic #0. When a 404 error is returned by xkcd, the embed will return the following code: 
{% highlight html %}
<span><a href="http://xkcd.com/0">xkcd-0</a></span>
{% endhighlight %}

If a comic was found, the image is returned within the html figure element. This figure has class "xkcd-embed" and contains the image of the comic as a link to the included page. As well as the title (or mouseover) text of the comic.
{% highlight html %}
<figure class="xkcd-embed">...</figure>
{% endhighlight %} 
For the title of the comic and the attribution, the figcaption element is used. Which includes the comic title with suffix:  " - created by xkcd", where xkcd is a link to xkcd.com. 
{% highlight html %}
<figcaption>"title" - created by <a href="http://xkcd.com">xkcd</a></figcaption>
{% endhighlight %} 



That is all there is to it. Let me close this second meta post of with another embedded xkcd comic: {% xkcd 917 %}
