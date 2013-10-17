---
layout: post
author: tim
title: Building the new Nubis site
categories: nubis
tags: [jekyll, nubis]
image: new-site.png
---

You may have noticed that Nubis has launched a new shiny website recently, on a new shiny domain: [nub.is](http://nub.is)
In this post, I'll list some of the technologies that we used to build the new site and some of the problems that we encountered.

If you're interested in the code, you can find it on GitHub: [nubisonline/nubis.com](https://github.com/nubisonline/nubis.com).
I'll probably change the repo name to nubisonline/nub.is pretty soon and forget to change this link so if you get a 404 you know what to try next.

First things first: the site was designed to be a one-page site.
It's pretty hip to have on of those nowawdays so who are we to stay behind?
This does raise a few interesting problems though:
If your whole site is one page, every visitor will download the entire website on each visit, even if it's just to check out the header image.
How are you going to make sure that your one page loads fast enough to not cause any annoyance?

There's plenty more where those came from. Read on for some of the problems we faced and the solutions that we came up with.

# Problems

Err, I mean challenges of course, there's no such thing as problems. Also synergy, retina and big data.

## Page loading time

This was already quite a though one.
Our designers picked a few nice photos for the background of the page header.
We needed to make sure that these were the first to load without compromising image quality.

Because the site had to work on laptops/desktops (read on for mobile) of all different sizes, there's a lot of code that resizes certain elements depending on the size of the users browser window.
A few of the elements that need this resizing (the nav bar height and the header height) are visible before the 'fold'.

Thus, we had to make sure that the code for all visible elements would run as fast as possible but _without_ blocking the loading of the page.

In the end we wrote a couple of CSS rules that set an approximate right size for most elements (and set the right size for a popular browser window size for the elements were an approximation in CSS was impossible).
The order of loading is:

1. The stylesheet
2. The HTML
3. The JavsScript (which executes in the order the elements have on the page)

It's still not perfect, but the site is pretty snappy now and all images look great.

## Responsiveness

These days, you just can't get away with not having a mobile-compatible site.
The best way for a site to be compatible with mobile devices is to make all pages *responsive*.

That means that the page layout will change based on how large the browser viewport is.

The problem here was that with all the animations our designers came up with, *it just wasn't possible* to use the same base HTML for both desktop and mobile devices.
I really did not want to make a separation but after some testing it turned out that even the newest devices were having trouble with the site.

The biggest culprit here is the code that runs every time that you scroll.
It's there for the parallax effect in the header and the size and active section of the navigation bar.

We played with the idea of adding some extra code that would load the right JS based on the viewport size but that would just make the page load time longer for both desktops and mobile.

After some debating, we split the site into two versions, one for desktop and one for mobile.
Which one is served depends on the user agent that the client sends (yuck :().
The good news is that the mobile site (I think of it as a 'light' version of our site) is fully responsive from the smallest phone to the biggest tablet.

## Analytics

Another problem with one page sites is that you can't really determine how many views each separate section of the site has had.

Our solution here is to just manually push a page view whenever someone scrolls 'into' a page.
We use Google Analytics and they expose this in their JS API.
Sure, it isn't perfect, as someone that wants to read our blog will always generate a page view for services, cases and about.
But hey, it's better than nothing. Our analytics guys agree.

## Search engines

Putting all of your content on one page goes against allmost everything you learn in SEO school.
However, Google has gotten pretty smart of the the years and if you use the new semantic HTML5 content tags (which we do, of course), it's not that bad.

I'll write a separate article on how we set everything up to help Google index our website better soon.

(Note to future self: insert link here)

# Architecture

Enough about the difficulties, let's talk about how we did it!

As you know by now, the site was split into two pieces with more or less the same content.
While we were actively (and hastily!) developing, these two parts were just that: copies with some differences that grew apart as time went by.
Of course, this form of coupling is incredibly harmful.
If a part of the content changes, you have to make this change at two different places.
This becomes an even bigger problem if you want to use the content at yet another place (see my future post about SEO on nub.is for more on that).

As soon as the deadline was past and the initial version of the site went live, I started cleaning up the site.
I started looking for a flexible templating system that gave me complete structural freedom, that would just replace some variable with text from a file.

I have quite some experience with Jekyll but since I wasn't sure how we would be handling blog posts through the site at that time I tried to find something geared less to blogging.
I found a few good frameworks/templates/standards but most of them enforced a certain way of working, something I wasn't really comfortable whith since I had most of the HTML finished.
After a while I gave up and just went with Jekyll.

I wrote a small [plugin](https://github.com/nubisonline/nubis.com/blob/master/_plugins/load_yaml/load_yaml.rb) that loads in YAML config files and appends them to the global site variable.
That way, I didn't have to put all of the config in the global `_config.yml` file, where it doesn't belong in my opinion.

It took some cleaning up but I'm quite happy with the result as it is now (although I'm still moving some code from client-side JS to precompiled HTML right now).
The full and light versions of the site are generated from a different HTML template that use the same content and I managed to import most of out old blog posts to Jekyll.

# The base HTML

Let's back up a bit now.
As I mentioned before, we started with the HTML for the full site.
The HTML for the mobile site was derived from that and now both of those sets of HTML are generated using a template and content.

Of course, this isn't the most efficient way to work on a project but when we started prototyping the deadline suddenly moved closer.
You know how these things are :).

We use the HTML5 editor's draft, mostly because of the added value of semantic tags like `<article>` and `<section>`.
For the hexagons, we use SVG clip-paths.
These only work in very modern browsers but this way works *so* much better than any alternative.
We're considering adding a fallback but our hexes are only for visual/design purposes and they fail quite graciously so it's not very nessecary.
The photographs next to the quotes also use this technique.
(In fact, the photo's next to the quotes are actually just the same as the team photo's)

You might have noticed the 'parallax' effect that our header has.
This is in fact just a fixed background image with scrollable content placed over it.
For added effect, we move the background up with half a pixel for every pixel that the user scrolls down.

&#x20;{% mention remco %} worked on the animations of the hexagons in the services section.
These don't use a clip-path since that would be a lot harder to animate in this way.
He built up the hexagons out of three parts that he animates separately using jQuery to achieve the result that you see.

The map that you see at the very bottom of the page, behind our contact form, is an actual functional Google Map.
The color comes from the custom styling that we ue to initialize the map, which is just a saturation of -100.

# Conclusion

And that's nub.is!
All in all the process was quite hectic but we're very happy with the end result, code-wise and looks-wise.
If you have any questions, suggestions or hatemail regarding the site, please don't hesitate to contact us.

