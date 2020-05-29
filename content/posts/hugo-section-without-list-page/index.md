---
title: "Hugo Section Without A List Page"
date: 2020-05-28T14:29:46-04:00
draft: false
tags: ["Hugo"]
categories: ["Technical"]
---

# Unpacking the problem


I was facing a problem when organizing my content. I had four files: about.md, coursework-overview.md, resume.md, and quotes.md. They were all related to "about" things, so I put them all in a "content/about/" folder. There were two things I didn't like about this setup:

1. I didn't like a url that went "about/about"
2. I didn't want to have a list page for these "about" related pages.

I explored the idea of leaving them in the base content/ directory, but I didn't like the clutter and this made it less streamlined to apply common layouts to these page. I began looking for another solution.

## Understanding Hugo Page Bundles

Hugo has two main options: [leaf bundles and branch bundles](https://gohugo.io/content-management/page-bundles/). Leaf bundles are for bundling a single page together with its resources (i.e. images, pdfs). Branch bundles are for building content around a list page.

The problem is that I was looking for a third option. I wanted to group content together, but didn't want to use a list page. This is similar to leaving content in the content/ directory, except it allows me to define a single layout to apply to these pages.

# Potential Solutions

Unfortunately I didn't find a natural solution, but I identified three slightly hacky versions.

## 1. Deviate from intended use of list.html aoiewjf 

Insert my about page into the list layout template, even if this is not the intended purpose of the list template. This would give a short "about" url for the about page and do away with the traditional list page.

## 2. Make use of headless mode

Set permalink of "about" to ":filename" in order to map about/about/ to about/ (this also removes "/about" from other content in the about/ directory). Use "headless: true" in the frontmatter of _index.html in order to prevent list page from being rendered in place of about single page.

One caveat is that by setting headless to true for "about/_index.html", hugo will not render any page at the link "/about". In order to get around this I moved the directory from "about" to "_about". This isn't a problem for the url's because permalink is set to ":filename" which means the section is never in the url anyways.

## 3. Stop obsessing

It's always worth taking a step back and considering practical/necessary an implementation is. In this case I wouldn't have very much to lose by leaving an unnecessary list page and using the url "/about/about/".

# Executing

In the end I went with solution number 2. It works well and checks all the boxes, even though it took more time to implement than solution number 3.

# Conclusion

While my implementation works, I look back and conclude that maybe I shouldn't have obsessed as much. While I learned a lot about Hugo templating in the process, I am trying to avoid going down these rabbit holes when easier implementations with comparable results are available. That being said, hopefully my experience will be helpful to someone in a similar place.

