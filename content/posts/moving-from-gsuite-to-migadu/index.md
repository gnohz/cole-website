+++
title = "Moving From GSuite to Migadu"
date = 2020-08-27T00:22:00-04:00
tags = ["email-hosting", "email", "gsuite", "migadu"]
categories = ["Productivity"]
draft = false
+++

Like many things, moving to Migadu from another email hosting provider is a two step process:

1.  Deciding that Migadu is the right option for you
2.  Making the switch


# Why Migadu {#why-migadu}

Just recently, Migadu went through a [redesign](https://www.migadu.com/blog/redesign/). Here are some helpful links for learning about their services:

-   [Pricing](https://www.migadu.com/pricing/)
-   [Pros/Cons](https://www.migadu.com/procon/)

The points that stuck out most to me:

1.  An account can have unlimited mailboxes on a domain at no additional charge.
2.  There is a soft limit of 5 registered domains on the micro plan
3.  Competitive Pricing

Combining these points, and you're telling me that with the micro plan I get unlimited mailboxes, 5 domains, all for a quarter of the price of a single domain on GSuite? Sign me up! I expect this feature to come in super handy when setting up email accounts on additional domains for side projects.


# Making the Switch {#making-the-switch}


## Setup {#setup}

1.  Sign up for Migadu and configure DNS
2.  Create a mailbox for yourself


## Migration {#migration}

There are several options for migrating mail to migadu. Some popular options are shared [here](https://web.archive.org/web/20190602203512/https://www.migadu.com/en/guides/mailtransfer.html).

Personally, I let Luke Smith's [mutt-wizard](https://github.com/LukeSmithxyz/mutt-wizard) do most of the heavy lifting. The steps for me were to:

1.  Add my new migadu account using mutt-wizard, indicating that I want to store all of my mail locally.
2.  Navigate to \`~/.local/share/mail\` and copy the contents of my previous mail folder my new migadu account folder. This involves changing the names of Gmail specific mailboxes to Migadu mailboxes. i.e. from "[Gmail].All Mail" to "Archive". I also followed [this guide](https://aaronweb.net/blog/2014/11/migrating-mail-between-imap-servers-using-mbsync/) to strip the mbsync metadata before sending the mail to the Migadu server.
3.  Run \`mw sync\`

That's it!


# Enjoy the benefits of Migadu {#enjoy-the-benefits-of-migadu}

I lied, there's actually a third step in the process of moving to Migadu: enjoy!

I'm most looking forward to avoiding the headache of setting up domain specific email accouts for side projects. Exciting times lay ahead.
