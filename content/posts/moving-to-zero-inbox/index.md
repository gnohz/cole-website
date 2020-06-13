---
title: "Ultimate Guide to Implementing Zero Inbox via Gmail Imap"
date: 2020-05-10T01:07:28-04:00
draft: false
tags: ["email", "imap", "zero inbox"]
categories: ["Productivity"]
---

# What is Zero Inbox Anyways?

The goal of "Zero Inbox" is not necessarily to remove all emails from the mailbox. In and of itself this would have a relatively small effect on daily life. Merlin Mann, the coiner of the term, puts the real meaning of inbox zero best:

> “It’s about how to reclaim your email, your attention, and your life. That “zero?” It’s not how many messages are in your inbox–it’s how much of your own brain is in that inbox. Especially when you don’t want it to be. That’s it.” – Merlin Mann

That being said, leaving no emails in your mailbox *can* be helpful for getting your brain out of your mailbox; it's just not all there is to it. Literal zero inbox offers the benefit of no longer having to worry about whether or not there is an email you are forgetting, or if an email slipped by your view.

In this way I see zero inbox as similar to GTD. You're brain is not meant for storing information, but decision making. Build your email habits to reflect this.

# Taking the First Steps

## Imap Complications

{{< comment >}}
I use neomutt with help from Luke Smith's [mutt-wizard](https://github.com/lukesmithxyz/mutt-wizard "Link to Luke Smith's mutt-wizard GitHub repo") for checking my mail, although you don't have to use this client for the following to apply. I have three accounts linked to it, reasons for which I elaborate on in ["making the most of email"](https://colekillian.com/post/making-the-most-of-email/). 
{{</ comment >}}

I began this transition with about 10,000 mail in my inbox across my three Imap mailboxes. The process should be as simple as moving all my mail from the inbox to the archive; how hard could that be?

As it turns out, this was more difficult than I anticipated, partially related to using Imap as opposed to the gmail web interface. When I tried to move all the mail in my inbox to the archive, I ended up moving it all to the trash instead :). Luckily this wasn't too hard to fix.

<details style="margin-bottom: 2.5em">
  <summary>How to fix if this happens to you</summary>
  This meant I had to go into gmail and move it to the archive. But there isn't a "move all trashed mail to archive" button. Instead the strategy is to first tag all the mail with a temporary category (I used "trashtoarchive"). Then restore all the trashed mail to the inbox (there is a button for this). Then archive all the mail in the inbox with the tag "trashtoarchive" (there is also a button for this).
</details>

## Understanding how Gmail handles Imap

### Gmail doesn't actually use folders for categorizing mail. 

Instead it uses tags, while keeping every email in the "All Mail" folder. This means that emails in your inbox are simply those with an "Inbox" tag applied, and that "moving to archive" is actually just "removing the inbox tag". 

However, Imap relies on a folder system, so a conversion must take place between these two schemas when syncing. Gmail approaches this by creating an Imap folder for each tag, and instructing an Imap client to download every email as many times as there are tags assigned to it (folders for it to stored in). 

{{< figure src="gmail-imap-structure.png" >}}

### There isn't a natural 'Archive' button

As a byproduct of the conversion that takes place between folder and tag format, there isn't a built-in endpoint for removing the inbox tag through Imap; instead you can communicate the same information (with the right settings) by deleting the message from the inbox folder, while leaving it in the All Mail folder.

The right settings are necessary to instruct gmail to move "deleted" mail to archive instead of actually deleting it. Set auto expunge to off and set the "When a message is marked as deleted and expunged from the last visible Imap folder" option to "Archive the message" (these settings are controlled in the gmail web inteface).

{{< figure src="imap-setting.png" >}}

### There isn't a natural 'Mark as Read' button with archiving (same problem when using gmail inerface)

Deleting a message from inbox folder doesn't mark it as read in the "All Mail" folder. There's no easy workaround, but you can solve this with a [script](https://lifehacker.com/automatically-mark-all-archived-email-as-read-with-a-gm-1704024261 "script for marking archived mail as read") originating from Whitson Gordon which marks all messages in the archive as read.

## Home Stretch. Getting your brain out of the inbox.

After clearing my inbox, I was ready to start freeing up brain memory allocated to email management. At first thought, the best email settings might be those that filter the most email out of your inbox for you. However, this is based on the more superficial meaning of inbox zero whereby your goal is to see that "0" and be done. In order to truly remove your mind from email, it is best to disable email filtering, and give yourself the responsibility of deciding which messages are deserving enough to make it to your inbox.

This means turning off gmail "Categories" (Social, Promotions, etc). With these categories turned on, part of your brain is occupied with trusting Google to categorize your emails correctly. For most people, these categories are used for filtering out mail that they don't want to read, but this is counter productive in that it leaves room for a voice at the back of your mind to wonder if you missed an important email. 

The proper way to filter out email that you don't want to read, is to take care of it yourself. Yes, it is actually possible to unsubscribe to emails that you don't want to receive. Websites like [unroll.me](unroll.me "unroll.me") make this process easier than ever before. For me, taking email filtering into my own hands has made a huge impact on getting my brain out of the inbox as I don't have to entertain the possibility of an email slipping by without my notice. 

It took me under a month to get to the point where I rarely receive emails I need to unsubscribe from. It's such a great feeling when you are actually interested in reading a large majority of the emails that make it to your inbox.

# Conclusion

Overall I am quite happy with the steps I have taken to get my brain out of my inbox. I am already feeling less "anxious" about checking mail, knowing that I don't have to worry about any emails slipping through the cracks.

Thanks for tuning in! Let me know if you have any suggestions or questions. Have a great day.

{{< comment >}}
  - Outlook handles Imap differently and actually has an "archive" folder. This means you have to approach the two systems differently which is unfortunate and expected.
{{</ comment >}}
