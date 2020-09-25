+++
title = "Syncing Org Files to Dropbox - Access Them From Anywhere"
date = 2020-09-18T09:00:00-04:00
tags = ["org-mode"]
categories = ["Productivity"]
draft = false
+++

By Adding org files to the cloud, you can access them from anywhere (yes, even when you're not at your computer!). This is very useful, especially for accessing org files from a mobile device.


# Setting Up Dropbox {#setting-up-dropbox}

Create a [dropbox](https://www.dropbox.com/h) account and install it on your computer. The Dropbox free tier gives 2GB of storage. On arch you can install with `yay -S dropbox`.

By default, dropbox will create a folder at `~/Dropbox`. Now we need to connect the agenda files to this folder. We can do this with a symlink!

You can have symlinks that link to items both in and outside of your Dropbox account; however, these two types of symlinks sync differently. See [Dropbox symlink help](https://help.dropbox.com/installs-integrations/sync-uploads/symlinks).

-   If you create a symlink that links to an item in your Dropbox account, we’ll sync the the symlink file at its location and the item that it links to at its location respectively
-   If you create a symlink that links to an item outside of your Dropbox account, when you sign in to dropbox.com you’ll only see the symlink file but not the content it links to

This means that we actually have to move the agenda files into the `~/Dropbox` folder, and then create a symlink back to their original location. For me this was easy because because I have a folder designated to storing my agenda files.

```bash
mv ~/.org/agenda/ ~/Dropbox
ln -s ~/Dropbox/agenda ~/.org/agenda
```

There you go, now your org files are synced to your dropbox and can be accessed from anywhere with an internet connection.


# What Next? {#what-next}

Now that your files are synced to Dropbox, it's super easy to get started with a mobile org program like `Organice` or `Orgzly`. For `Organice` you navigate to [https://organice.200ok.ch/](https://organice.200ok.ch/) in a web browser, sign in, and you're done. You can learn more about the differences between `Organice` and `Orgzly` [here](https://colekillian.com/snippets/comparing-organice-and-orzly/).
