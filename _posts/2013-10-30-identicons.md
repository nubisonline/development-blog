---
layout: post
author: tim
title: Nubis identicons
categories: programming
tags: [identicons, nubis]
image: identicon.png
excerpt: "<p>Some time ago, GitHub retired the default cat icon and introduced <a href='https://github.com/blog/1586-identicons'>Identicons</a>. While we were working on the <a href='http://nub.is/nonexistent'>nub.is 404 page</a>, <span itemscope itemtype='http://schema.org/Person'><meta content='Walid Said' itemprop='name'><meta content='Senior Developer' itemprop='jobTitle'><a href='https://plus.google.com/104993994538898250860' itemprop='url'>Walid</a></span> replaced the blocks in his GitHub identicon by Nubis blocks (from our cloud icon) and uploaded it to Gravatar. That (and the fact that we just got a new server by WebFaction, which I might blog about later) inspired me to make Nubis identicons.</p>
---

Some time ago, GitHub retired the default cat icon and introduced [Identicons](https://github.com/blog/1586-identicons).
While we were working on the [nub.is 404 page](http://nub.is/nonexistent), {% mention walid %} replaced the blocks in his GitHub identicon by Nubis blocks (from our cloud icon) and uploaded it to Gravatar.

<img alt="Identicon Walid" src="https://identicons.github.com/walidsaid.png" style="width: 48%">
<img alt="Gravatar Walid" src="https://2.gravatar.com/avatar/54358d53ae6b7e695b63201f3af554bb?d=https%3A%2F%2Fidenticons.github.com%2Ff477d49569195dd32c8c71edb4feba06.png&r=x&s=400" style="width: 48%">

That (and the fact that we just got a new server by WebFaction, which I might blog about later) inspired me to make Nubis identicons.

They work similar to gravatar, in that you first get a hash for your user name and can then request an icon for your hash.
Of course, I had to one-up GitHub, so our identicons are 3D! (This might also be because the blocks are identicons)

You can check out the [full source code on GitHub](https://github.com/nubisonline/identicons), but here are the highlights.
I have two classes, `Identicon` and `Icon`. `Icon` represents an actual image, in Imagemagick. `Identicon` represents the identicon.

I chose to use Sinatra to handle the HTTP, which is beautifully succinct:

{% highlight ruby %}
get '/:hash.png', :provides => :png do |hash|
	icon = Identicon::from_hash hash

	icon.to_png
end

get '/:hash.coords', :provides => :text do |hash|
	icon = Identicon::from_hash hash

	icon.to_coord
end

get '/:hash.json', :provides => :json do |hash|
	icon = Identicon::from_hash hash

	icon.to_a.to_json
end

get '/:name' do |name|
	icon = Identicon.new name

	icon.to_hash
end
{% endhighlight %}

As you can see, HTTP methods map to model methods quite clean-ly.

Practically, our icons probably aren't great because of collisions.
I reduce the SHA-2 (which is quite collision-proof) hash which has 256 bits of data to 64 bits of data (the 64 blocks in the picture).
Furthermore, some icons will not be distinguishable from each other because of the way you look at them.

But hey, the pictures are kind of pretty!

<img src="http://id.nub.is/aaaa00000000000000aa0000000000000a00000000000000aaaa000000000000.png" style="width: 11em;" alt="N">
<img src="http://id.nub.is/aaaa000000000000a000000000000000a000000000000000aaaa000000000000.png" style="width: 11em;" alt="U">
<img src="http://id.nub.is/aaaa000000000000a0a0000000000000aaa00000000000000000000000000000.png" style="width: 11em;" alt="B">
<img src="http://id.nub.is/aaaa000000000000000000000000000000000000000000000000000000000000.png" style="width: 11em;" alt="I">
<img src="http://id.nub.is/aaaa000000000000aa0a000000000000aa0a000000000000aa0a000000000000.png" style="width: 11em;" alt="S">

