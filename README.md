# Nubis Development Blog

## About

This blog will first and foremost be a place for members of the Nubis development team to show off their knowledge and experience.
All of the code will be hand-crafted based on the mojombo/jekyll static site generator.
Its contents includes, but is not limited to, tutorials, opinions, code samples, meta posts about this site and basically anything that a member of the Nubis dev team might find interesting.

## Contributing

The Nubis Development blog is completely open source, both code and content.
Actually, most of it is even free.

So, if you find anything that's not 100% right with the blog, you are free to improve it.
If you do, we would love it if you would contribute that fix back to us.

We have a few guidelines.
Apart from the motivation to do so, there isn't much of a difference between contributing for Nubis developers and external developers that might be just interested.

Only senior Nubis developers have push access to this repository and nothing should be committed directly here.
We have two separate branches for staging, one for content and one for code.

![The branching model](http://nubisonline.nl/files/dev-blog/branches.png)

If you are going to work on something, please make sure you do it in your own fork and then make a pull request to the right branch.
We cannot merge in requests to the wrong branch and we will close them immediately.

`gh_pages` is the live branch that is hosted on http://development.nubisonline.nl/.

### Code

Please make sure that any HTML you write follows our vision for this website.
We want to use clean, meaningful (`<article>`, `<section>`, `<header>`, etc.) HTML5.

Every page that represents something in the real world should have [Schema](http://schema.org/) meta-data and articles should always have (valid) author information.

Any programmatic contribution must come in the form of a Jekyll plugin.
If your plugin is big or it could be generalized so other projects can use it, please consider creating a new project and including it in this one as a git submodule.

### Articles

Although we are not really looking for article contributions outside of Nubis, we're open to anything.
If you have a great article that you think would fit this blog, don't be shy and let us know.

## Licensing

Unless otherwise indicated, both content and code are GPLv3-or-later.
