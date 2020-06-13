---
title: "Making a Home Direct Backup the Simple Way"
date: 2020-06-08T00:32:39-04:00
draft: false
tags: ["linux"]
categories: ["Technical"]
---

After all the configuring you may do to your linux machine, it would be sad to see it all go down the drain in some unforseen event. This is where making a backup of your home folder can come in handy. It's not quite an ISO that you can boot off of, but if you are trying to reference certain config files in the future you'll know where to look.


# The Simple Way: Tar

It's as easy as one tar command.

1. First plug in your external harddrive.

2. Run `lsblk` to see which dev identifier it has your harddrive was assigned.

3. Use `mount` to mount the harddrive so that you can start interacting with it. In my case my harddrive was at `/dev/sba1`, and I chose to mount it at `/mnt/Toshiba`.

4. Run `tar` with `--exclude` flags in order to avoid a slowdown copying over unimportant information. Make sure to check out the man page. `-c` indicates creating a new archive. `-v` makes it verbose so that you can see what's being copied over. `-z` filters the archive through gzip. `-f` indicates where to save the new archive. The last argument indicates which directory to make an archive of.

# Entire Command

{{< highlight bash >}}
lsblk

sudo mount /dev/sba1 /mnt/Toshiba/

tar -cvz \
--exclude="VirtualBox VMs" \
--exclude="node_modules" \
--exclude="venv" \
--exclude=".cache" \
--exclude=".debug" \
--exclude="Applications" \
--exclude="google-cloud-sdk" \
--exclude=".zoom" \
--exclude=".nuget" \
--exclude="styleImages" \
--exclude=".android" \
--exclude="mini-kube" \
--exclude="Steam" \
-f /mnt/Toshiba/06-07.tar.gz /home/gautierk
{{< /highlight >}}

All done! Make sure to test it out in order to verify that everything worked as you expected. It's not fun to find yourself in a situation where you'll be relying on your home backup, only to find out that your previous attempt isn't going to cut it.

# Next Steps

I may look into more advanced backup solutions in the future (maybe cronjobs), but until then this was enough to help me sleep better at night :)

