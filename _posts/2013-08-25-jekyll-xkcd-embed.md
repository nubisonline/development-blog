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

That is all there is to it. Let me close this post of with a second embedded xkcd comic: {% xkcd 917 %}
