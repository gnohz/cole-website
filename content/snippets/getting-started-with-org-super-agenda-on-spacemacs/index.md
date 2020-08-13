---
title: "Getting Started With Org Super Agenda on Spacemacs"
date: 2020-08-13T08:19:48-04:00
draft: false
tags: ["spacemacs", "emacs", "org", "org-agenda"]
categories: ["Productivity"]
---

# Demo

I recently got setup with [org-super-agenda](https://github.com/alphapapa/org-super-agenda). It's a great tool that makes custom agenda views way easier to manage.

 I use a "Next View" for quickly finding a task to work on. This is what it looks like:

{{< image src="org-super-next.png" caption="Super Next View" >}}

I use a "Todo View" for finding tasks to mark as `NEXT`. This is what it looks like:

{{< image src="org-super-todo.png" caption="Super Todo View" >}}

# Setup

Setting up with Spacemacs was quick. Add `org-super-agenda` to `dotspacemacs-additional-packages`


{{< highlight elisp >}}
dotspacemacs-additional-packages '(
                                   org-super-agenda
                                   ...
                                   )
{{</ highlight >}}

Next add your custom agenda commands to `dotspacemacs/user-config`:

{{< highlight elisp >}}
(setq org-agenda-custom-commands
      '(("n" "Next View"
         ((agenda "" ((org-agenda-span 'day)
                      (org-super-agenda-groups
                       '((:name "Today"
                                :time-grid t
                                :todo "TODAY"
                                :scheduled today
                                :order 0)
                         (:habit t)
                         (:name "Due Today"
                                :deadline today
                                :order 2)
                         (:name "Due Soon"
                                :deadline future
                                :order 8)
                         (:name "Overdue"
                                :deadline past
                                :order 7)
                         ))))
          (todo "" ((org-agenda-overriding-header "")
                    (org-super-agenda-groups
                     '((:name "Inbox"
                              :file-path "inbox"
                              :order 0
                              )
                       (:discard (:todo "TODO"))
                       (:auto-category t
                                       :order 9)
                       ))))))
        ("t" "Todo View"
         (
          (todo "" ((org-agenda-overriding-header "")
                    (org-super-agenda-groups
                     '((:name "Inbox"
                              :file-path "inbox"
                              :order 0
                              )
                       (:auto-category t
                                       :order 9)
                       ))))))
        ))

(org-super-agenda-mode)
{{</ highlight >}}

Now you're good to go!



