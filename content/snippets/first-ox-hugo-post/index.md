+++
title = "First ox-hugo post"
date = 2020-08-24T21:51:00-04:00
tags = ["org-mode", "hugo"]
draft = false
weight = 2001
+++

# This is Awesome! {#this-is-awesome}

I just installed [ox-hugo](https://github.com/kaushalmodi/ox-hugo), an org extension that makes it easy to export org files as hugo posts.


# Setting Up {#setting-up}

On spacemacs it was as easy as adding `ox-hugo` to the list of `dotspacemacs-additional-packages`, along with adding the following to user-config:

```elisp
(use-package ox-hugo
  :ensure t          ;Auto-install the package from Melpa (optional)
  :after ox)
```

Then you're good to go. I'm excited about the way it makes it easy to write posts via org-capture, but I should really be spending more time writing and less time configuring emacs (:

Even though it's easy to setup, I recommend reading through the entire [ox-hugo](https://ox-hugo.scripter.co/) documentation, and this [blog post](https://www.shanesveller.com/blog/2018/02/13/blogging-with-org-mode-and-ox-hugo/) for more specifics on the workflow.


# Extra Goodies {#extra-goodies}

I recommend setting up auto-export on saving, org-capture setup.

Personally I like using hugo leaf bundles for all my hugo posts because it helps keep the images organized.


# Have fun! {#have-fun}
