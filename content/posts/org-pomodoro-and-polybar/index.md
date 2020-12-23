+++
title = "Org Pomodoro and Polybar"
date = 2020-12-23T11:19:00-05:00
tags = ["org-mode", "pomodoro", "polybar"]
categories = ["Productivity"]
draft = false
+++

# The Pomodoro Technique {#the-pomodoro-technique}

The pomodoro technique is a time management system where you do focused work for a set period of time (usually 25 minutes), take a quick break (usually 5 minutes), and then start again. It's great for boosting productivity because you can hold yourself accountable to finishing a pomodoro once you start it.

It was about time to get pomodoro working with org-mode. Luckily there's an awesome package, [org-pomodoro](https://github.com/marcinkoziej/org-pomodoro), that let's you do just that.


# How The Package Works {#how-the-package-works}

There's only one command exposed by the org-pomodoro package: `org-pomodoro`. If you don't have an active pomodoro, calling `org-pomodoro` will start a timer (25 minutes by default) and clock in to the currently focused task. Once the timer runs out, a noise will be played and you will be clocked out of your task. 5 minutes later, another noise will be played to let you know that the break is over. It's then up to you to start another pomodor by calling `org-pomodoro` again.

If you call `org-pomodoro` when you already have a pomodoro going, you will be prompted with whether or not you want to stop your running timer.

On spacemacs the keybindings for `org-pomodoro` are `SPC m C p` or `, C p`.


# Screenshots {#screenshots}

{{< image src="./org-pomodoro.png" caption="When pomodoro timer is running" >}}

{{< image src="./org-pomodoro-cancel.png" caption="After cancelling a pomodoro timer" >}}


# Customization {#customization}


## Notifications {#notifications}

The alert noise is nice, but doesn't always catch my attention, especially if I have my computer muted.

Luckily it's easy to customize org-pomodoro to use libnotify so that you get visual notifications alongside the auditory ones:

```elisp
(use-package org-pomodoro
  :ensure t
  :commands (org-pomodoro)
  :config
  (setq
   alert-user-configuration (quote ((((:category . "org-pomodoro")) libnotify nil)))
   ))
```


## Pomodoro Length {#pomodoro-length}

A common alternative to the 25 - 5 pomodoro settings is 50 - 10. I find that 50 - 10 works better for me. Again it's a quick change:

```elisp
(use-package org-pomodoro
  :ensure t
  :commands (org-pomodoro)
  :config
  (setq
   org-pomodoro-length 50
   org-pomodoro-short-break-length 10
   ))
```


## Connecting to Polybar {#connecting-to-polybar}

At this point we have a working org-pomodoro, but we can see the remaining time on the timer when we are using emacs. Believe it or not sometimes I am working outside of emacs, but I still want access to the time left. We can use emacsclient to make this happen:

```elisp

(defun ruborcalor/org-pomodoro-time ()
  "Return the remaining pomodoro time"
  (if (org-pomodoro-active-p)
      (cl-case org-pomodoro-state
        (:pomodoro
           (format "Pomo: %d minutes - %s" (/ (org-pomodoro-remaining-seconds) 60) org-clock-heading))
        (:short-break
         (format "Short break time: %d minutes" (/ (org-pomodoro-remaining-seconds) 60)))
        (:long-break
         (format "Long break time: %d minutes" (/ (org-pomodoro-remaining-seconds) 60)))
        (:overtime
         (format "Overtime! %d minutes" (/ (org-pomodoro-remaining-seconds) 60))))
    "No active pomo"))
```

Now we can run `emacsclient -e '(ruborcalor/org-pomodoro-time)'` from the terminal and get the remaining time on the org-pomodoro timer.

To connect it to polybar, create the following shell script:

```sh
#!/bin/sh

underline_color="%{u#99c2ff}%{+u}"
pomo_message=$(emacsclient -e '(ruborcalor/org-pomodoro-time)' | cut -d '"' -f 2)
echo ${underline_color}${pomo_message}
```

and then add it to your polybar config (remember to include `pomodoro` in your polbar modules):

```nil
[module/pomodoro]
type = custom/script
exec = ~/.local/bin/scripts/pomodoro-bar.sh
interval = 5
```


# Conclusion {#conclusion}

I hope this helps you get up and running with org-pomodoro. Let me know if you come across any further customization improvements!
