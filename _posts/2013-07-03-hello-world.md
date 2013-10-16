---
layout: post
title: "Hello world!"
published: true
---

A new site, a new look, a new blog!

Site is made with [kerouac](https://github.com/jaredhanson/kerouac), which seems like a nice static site generator. Tried a few different ones but settled on this as its configuration seemed quite simple. It is quite new, and may still require some more features. But it's simple, and straightforward.

I published one kerouac plugin, [kerouac-blog-index](https://github.com/ile/kerouac-blog-index), which creates an index page of the blog posts. Yes, the basic blog plugin didn't create an index, so I made this.

This site is hosted on [gh-pages](http://pages.github.com/) and the build process from kerouac code/data to the real site is done with [wercker](http://wercker.com/).

I will be informing here on new software that I have been making. Stay tuned...

## Update on 2013-10-16

Moved to [Jekyll](https://help.github.com/articles/using-jekyll-with-pages). Had a problem with Kerouac, and didn't want to fix that myself, so went the easier way with Jekyll. Also I don't need an external build tool (like wercker) with Jekyll since building done by GitHub. I lose some flexibility though. Also, [prose.io](http://prose.io/) is used as an editor.