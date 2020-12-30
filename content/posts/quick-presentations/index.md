+++
title = "The Best Way To Make Quick Presentations"
date = 2020-12-30T11:15:00-05:00
tags = ["org-mode", "presentations"]
categories = ["Productivity"]
draft = false
+++

# Overview {#overview}

There are three aspects to a slideshow presentation:

-   the content
-   the design
-   the delivery

This post focuses on the content and design. Imagine yourself, looking to quickly prepare a slideshow, and opening up google slides or powerpoint. What you really bring to the table is the content, but along the way you have to deal with manually composing slides; moving the textbox a little to the right, changing the font, resizing images.

Would it be possible to write a program that lets you forget about design, focus on the content, and then have a polished presentation generated for you? You would need a way to communicate the content with the program, an outliner tool. hmm

It turns out that there is a great package that accomplishes exactly this! See [org-reveal](https://github.com/yjwen/org-reveal), a package that uses [reveal.js](https://revealjs.com/) to turn plain text org files into html slideshow presentations. You can include all the typical presentation things: images, code, tables, etc. See an example presentation [here](https://revealjs.com/).


# Installation {#installation}

In spacemacs `org-reveal` is easy to setup. You enable it using org variables as follows and set the `org-re-reveal-root` to where you installed `reveal.js` on your computer:

```elisp
(org :variables
     org-enable-reveal-js-support t
   )

(setq org-re-reveal-root "file:///home/gautierk/.npm-global/lib/node_modules/reveal.js/")
```


# Usage {#usage}

To use org-reveal, outline your presentation in an org file. Use org metadata to set things like the title and author. When you're ready to export, run `org-export-dispatch` (`, e e` on spacemacs) and then type `v b` to export using reveal. You did it! For more options see the [org-reveal](https://github.com/yjwen/org-reveal) README. [Here](https://ruborcalor.github.io/Academic-Cluster-Presentation/) is an example presentation I made.
