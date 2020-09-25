+++
title = "Hacking Into My Computer After Getting Locked Out"
date = 2020-09-25T10:11:00-04:00
tags = ["linux"]
categories = ["Story"]
draft = false
+++

# Just a Normal Day {#just-a-normal-day}

I updated all the packages on my system with `pacman -Syyu`. Didn't think much of it at the time.

Later in the day my computer froze when using bash to execute a python script I was developing (whoops).

After waiting a minute to see if my computer would recover, I initiated force shutdown.

I started up my computer as normal, but this time I couldn't login! It was disheartening to be shown `Password Incorrect` over and over. Caps lock is off, check. Try a couple keyboards, check.


# Hacker Mode {#hacker-mode}

At first I was scared that the script I ran had somehow changed my password. Off to the newbie corner I go :). Uh oh, I hadn't read the patch notes. Turns out updates to `PAM` and `PAMBASE` might prevent login. Other people were having the same problem, all I had to do was login with rescue mode!

I follow the instructions to boot rescue mode. i.e. pressing `e` on the grub screen to edit the boot parameters and adding the kernel option `systemd.unit=rescue.target`. I find myself in a terminal prompted for my root password; what's my root password again? This can't be good. How can I possibly login without my root password?!

Back to the newbie forums. Luckily I find someone with a similar problem. He was having trouble logging into rescue mode because he had never set the root password in the first place. Turns out the solution is to reset your root password without knowing your root password. I couldn't believe how easy the process was.

Instead of appending `systemd.unit=rescue.target` to the kernel params, I appended `init=/bin/bash`. This tells the computer to run `/bin/bash` as init rather than the system init and puts you into a root shell without being prompted for a password. Your root file system is mounted as read-only now, so the first step is to mount it as read/write: `mount -n -o remount,rw /`. Then use `passwd` to create a new password for the root user, `reboot -f`, and you are good to go!

> If you want to "fix" this, lock GRUB and your BIOS with a password and put your hard disk first in boot order. If someone else has physical access and can put the (non-encrypted) hard disk into another computer, you have lost anyway

After that I was able to successfully boot into rescue mode and fix the problems with PAM. I did the following:

```bash
cd /etc/pam.d/
mv system-login system-login.backup
mv system-login.pacnew system-login
```

One final reboot to the computer, and here I am!


# Final Thoughts {#final-thoughts}

I was late for my 10 oclock shutdown because of this, but thank god I'm back. This could have been quite a disaster; I'll be backing up my pc tomorrow.

This is the first time I've ever had a "breaking change" with manjaro, but it was my fault and could have been prevented by reading the update notes. Live and learn.

Mentioned links:

-   [Why does linux allow init bin bash](https://unix.stackexchange.com/questions/34462/why-does-linux-allow-init-bin-bash)
-   [Can't login after update](https://forum.manjaro.org/t/cant-login-after-update/16231)
-   [Update notes](https://forum.manjaro.org/t/stable-update-2020-08-28-kernels-systemd-pam-pambase-kde-git-deepin-pamac-nvidia-450-66-libreoffice-7-0/16146/2)
-   [Cannot boot to rescue mode solved](https://forum.manjaro.org/t/cannot-boot-to-rescue-mode-solved/25636)
