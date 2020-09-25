---
title: "Making i3 the Best Window Manager with i3-swallow and i3-layout-manager"
date: 2020-06-22T21:56:08-04:00
draft: false
tags: ["linux", "i3", "wm", "ricing", "computer-configuration"]
categories: ["Productivity"]
---

# Intro

Recently I gave dwm a try. I really enjoyed window swallowing and not having to worry as much about maintaining window layouts, but I could not work with the multiple monitor support. Luckily awesome people have written scripts/patches that bring these features to i3!

# Patches

## i3-layout-manager

This script makes taking advantage of i3's layout saving super easy. There was one layout I consistently found myself putting together. This script helps eliminate that friction as seen below. [Github](https://github.com/klaxalk/i3-layout-manager)

### Before
{{< image src="before.gif" >}}

### After
{{< image src="recorded.gif" >}}

### Install

Installation is very easy with the AUR.

{{< highlight bash >}}
# dependencies
yay -S perl-anyevent-i3
yay -S perl-json-xs
# script
sudo pacman -S libanyevent-i3-perl
{{< /highlight >}}

## i3-swallow

This one is pretty self explanatory. Very easy to install and does a great job. Make sure to install `i3ipc`. [Reddit Link](https://www.reddit.com/r/i3wm/comments/gziwmf/oc_i3_swallow_automatic/).

### Example

{{< image src="window-swallowing.gif" caption="Example of swallowing obs" >}}

### Install

{{< highlight bash >}}
# dependency
python -m pip install i3ipc --user
# script
curl https://gist.githubusercontent.com/windwp/b46e8bdeac793867b34d2191e66a6f44/raw/39fc471a6c8b4fcabf3b289f4b2bbd51c90fb1c7/i3-swallow.py -o ~/.local/bin/scripts/i3-swallow.py
chmod +x $_
echo "exec --no-startup-id python $HOME/.config/i3/i3-swallow.py" >> ~/.config/i3/config
i3-msg restart
{{< /highlight >}}

# Conclusion

I almost looked into trying bspwm, but didn't want to take the time to set everything up the way I have done with i3. I'm glad to have found these scripts. Off the top of my head I can't think of anything else I would want from a window manager that I'm not getting. Hopefully I'll be able to avoid spending time on customization for the next while.
