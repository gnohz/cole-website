+++
title = "First Thoughts On Org Roam"
date = 2020-08-24T22:49:00-04:00
tags = ["org-mode", "roam-research"]
draft = false
weight = 2002
+++

# Background {#background}

I find myself taking a lot of notes, and was looking for a place to store them.

[Roam Research](https://roamresearch.com/) is the big craze these days: "A note-taking tool for networked thought." It seemed like just what I was looking for. It's growing quickly and has a great community. I was going to give it a try when I happened upon [org-roam](https://github.com/org-roam/org-roam): "a Rudimentary Roam replica with Org-mode".

Org-roam seemed like the best of both worlds. I get the benefits of roam research, for free, all from spacemacs! It's in its early stages, but like roam research it is growing very quickly.

This is where my roam graph stands as of today (displayed by `org-roam-server`):
![](org-roam-graph-08-24.png)


# Installation {#installation}

Getting setup on spacemacs is straightforward. These are the relevant parts of my config for getting basic org-roam functionality:

```elisp
(with-eval-after-load 'org
      (use-package company-org-roam
      :ensure t
      ;; You may want to pin in case the version from stable.melpa.org is not working
                                        ; :pin melpa
      :config
      (push 'company-org-roam company-backends))

    (require 'org-tempo)
    (require 'org-protocol)
    (require 'org-roam-protocol)

  (use-package org-roam
      :ensure t
      :hook
      (after-init . org-roam-mode)
      :custom
      (org-roam-directory "/home/gautierk/.org/roam/")
      :init
      (progn
        ;; (spacemacs/declare-prefix "af" "org-roam")
        (spacemacs/set-leader-keys
          "afl" 'org-roam
          "aft" 'org-roam-dailies-today
          "aff" 'org-roam-find-file
          "afg" 'org-roam-graph)

        ;; (spacemacs/declare-prefix-for-mode 'org-mode "mr" "org-roam")
        (spacemacs/set-leader-keys-for-major-mode 'org-mode
          "fl" 'org-roam
          "ft" 'org-roam-dailies-today
          "fb" 'org-roam-switch-to-buffer
          "ff" 'org-roam-find-file
          "fi" 'org-roam-insert
          "fI" 'org-roam-insert-immediate
          "fg" 'org-roam-graph)))
  )
```

If you want to enable [org-roam-server](https://github.com/org-roam/org-roam-server), add the following in the `with-eval-after-load 'org` block and make sure emacs server is started:

```elisp
(use-package org-roam-server
  :ensure t
  :config
  (setq org-roam-server-host "127.0.0.1"
        org-roam-server-port 8080
        org-roam-server-authenticate nil
        org-roam-server-export-inline-images t
        org-roam-server-serve-files nil
        org-roam-server-served-file-extensions '("pdf" "mp4" "ogv")
        org-roam-server-network-poll t
        org-roam-server-network-arrows nil
        org-roam-server-network-label-truncate t
        org-roam-server-network-label-truncate-length 60
        org-roam-server-network-label-wrap-length 20))
```


# First Thoughts {#first-thoughts}

I'm probably still in the honey moon phase, so i'll have to revisit these thoughts later, but right now i'm having a blast. It's great that taking notes is as easy as `SPC a f f`, and it's very rewarding to see the org roam graph growing over time.

I will say that at this point I rarely look at a note i've taken in the past, but even so writing something down helps me think things out, and potentially improves my retention on a topic. Hopefully over time I'll capitalize more on the ability to look at historical notes.
