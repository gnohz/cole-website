---
title: "Discovering Safe Eyes"
date: 2020-08-14T23:29:25-04:00
draft: false
tags: ["ergonomics"]
---

I was just introduced to [safeeyes](https://slgobinath.github.io/SafeEyes/) by my brother.

It's a "Free and Open Source tool for Linux users to reduce and prevent repetitive strain injury (RSI)". It works by reminding you to do a 15 second eye exercise on a 15 minute interval. During the exercise your screen is blocked which so you have no choice but to take care of your eyes. This may sound a bit odd at first, but give it a try! You won't regret it.

{{< image src="screenshot.png" caption="Screenshot during eye exercise">}}

It's in the aur so installing is as easy as `yay -S safeeyes`. 

Then I added it to my `i3` config so that it starts on boot.

`exec --no-startup-id safeeyes`

That's all.

