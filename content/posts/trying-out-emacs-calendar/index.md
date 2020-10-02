+++
title = "Trying Out Emacs Calendar"
date = 2020-09-30T10:35:00-04:00
tags = ["org-mode"]
categories = ["Productivity"]
draft = false
+++

I was investigating emacs calendar options anc ame across [emacs-calfw](https://github.com/kiwanami/emacs-calfw/). It's a nice package for viewing calendar items from howm, ical, cal, or org in a google calendar like format.

I was mainly interested in org calendar. To get up and running clone the emacs-calfw repo and add it to your load path. then require then relevant files:

```elisp
(add-to-list 'load-path "/home/gautierk/.emacs-conf/emacs-calfw")
(require 'calfw)
(require 'calfw-org)
```

Now to display the calendar his `SPC SPC` and run `cfw:open-org-calendar`. You should see something like the following if you're on spacemacs:

{{< image src="./emacs-calfw-view.png" >}}

Press `W` to display the weekly view.

{{< image src="./emacs-calfw-weekly-view.png" >}}

It's awesome that this package exists, but too bad that it doesn't display rectangles positioned and sized based on the time and duration of the event in the same way google calendar does.
