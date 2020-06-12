---
title: "First Impressions of Larbs Coming From i3"
date: 2020-06-11T20:36:14-04:00
draft: false
tags: ["Linux", "dwm", "i3"]
categories: ["Technical"]
---

# Intro

You may have heard of [Luke Smith](lukesmith.xyz) and [LARBS](larbs.xyz) (Luke's auto ricing bootstrapping scripts); if not I suggest you check them out.

I've been following his YouTube channel for a while now. I feel like it gives me the opportunity to learn without fumbling around too much.

About a year ago I moved to Linux (Manjaro i3), but larbs has always peaked my interest. Today I decided to finally give it a try. dwm window swallowing was the last straw.

{{< image src="triplescreen.png" title="The Final Form" caption="The Final Form" >}}

# First Impressions

I couldn't have asked for a more seamless transition. I ran `sudo sh larbs.sh` and choose to have larbs do its magic on my main user (only after backing up my home directory). Upon rebooting, I selected `dwm` from the login window and I was good to go. All of my documents were just where I left them. The only files that get overwritten are config files (definitely backup your config files though).

First things first is to open up "A Friendly Guide to LARBS" with `fn+mod+1`. Moving around with dwm is definitely different from i3, but I became a fan right away. Main reason being how nice it was to be able to forget the need to manually size windows. Let the master slave model do the heavy lifting for you.

# Initial Modifications

While I was having a good time, I recognized I needed to change somethings.

## Multiple Monitors

I use two monitors in addition to my laptop screen, layout as follows:

{{< figure src="monitor_layout.png" title="Monitor layout as seen in arandr" >}}

I would assume that Luke rarely uses multiple monitors, as he bound the command for moving between then to the arrow keys! It wasn't too hard to assign this functionality back to the default bindings of `comma` and `period`. I had to make room by getting rid of some `mpc` commands, but I rarely use seeking commands anyway.

A big difference between dwm and i3 is in the way that dwm assigns each monitor its own set of tags, while i3 shares the workspaces between monitors. There are some pros and cons to each approach. While dwm's approach ensures that you never run out of tags, it also makes it a bit harder to move tag layouts between monitors. I did find a [dwm patch](https://dwm.suckless.org/patches/single_tagset/) that implements one list of tags for all monitors, but I'll stick with the distinct monitor approach for now.

## $BROWSER

Changing back to firefox. I have nothing against brave. I just really like tree style tabs (they make it much easier to manage a large number of tabs), and I find that they function far better on firefox than on Brave. If you have a good tree tabs solution for brave let me know!

## Statusbar

Polybar is something I very much would have liked to get up and running in place of the dwm status bar. I was quite dissapointed when I learned that, without a serious time investment, polybar wouldn't be a possibility.

I spent my fair share of timing ricing my i3 configuration, but I'm at the point where I moving in a more pragmatic direction and want things to just werk a little more.

Luckily this is more of an aesthetic preference than a functional one, so it isn't that big of a blow. I'll be leaving the dwm status bar for now. Luke's default widgets are pretty nice (that being said I already had most of them in my polybar config).

# Conclusion

I'll be sticking with dwm and larbs for at least the next two weeks to build up a solid feel for it. Already one day in I doubt that I'll be going back to i3.
