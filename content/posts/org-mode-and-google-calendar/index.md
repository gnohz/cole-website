+++
title = "Org Mode and Google Calendar"
date = 2020-09-27T17:46:00-04:00
tags = ["org-mode", "google-calendar"]
categories = ["Productivity"]
draft = false
+++

I've been having an internal debate over org mode and google calendar. Normally I manage the scheduling of my courses using Google Calendar, but would it be better to use org mode instead?


# Google Calendar Pros {#google-calendar-pros}

-   Great mobile support
-   Reply "Yes" or "No" to event invitations
-   Great display
-   Reminders
-   Share calendars with other people
-   Color coding


# Org Mode Pros {#org-mode-pros}

-   Time tracking
-   Note taking
-   Vim bindings
-   Less context switching
-   The benefits of org


# Deciding Questions {#deciding-questions}

Don't need a perfect system. Looking for something good. If it ain't broke don't fix it.

-   What am I missing by using org mode?

    I feel like I am missing shared calendars and a great weekly view.
-   Are shared calendars valuable?

    My understanding is that they practically become a necessity once entering the work force. Personally, however, I currently have no use for them.
-   Is a great weekly view valuable?

    It makes it easy to see conflicting events, and how they are spaced out throughout your day.


# Options {#options}


## Separated {#separated}

Use org mode for todos/deadlines and google calendar for scheduled events.

This allows you to get all the benefits of google calendar for scheduled events, and all the benefits of org mode for todos/deadlines. A potential downside is that this approach requires more context switching. You'll have to consult the org agenda and the google calendar at the same time in order to make sure that you aren't missing anything.

Time tracking becomes difficult, because if you want to time track an event from the calendar, you'll have to duplicate it into org. This might not be so bad though. My main events would be:

-   lectures
-   meetings

And it's not important to get fine grained time tracking to the specific meeting itself. A general task of `meetings` for a set of categories that I could clock to would suffice.


## Synced {#synced}

By using [org-gcal](https://github.com/myuhe/org-gcal.el), you can push events from org mode to google calendar, and pull them from google calendar to org mode.

In either case you get to take advantage of the google calendar view without giving up org mode, but there are some limitations to the integration.

-   repeating org events
-   coloring

You also have to deal with the potential headache of keeping everything synced up.


## Isolated {#isolated}

Use org mode for everything and don't use google calendar

There are solutions like `emacs-calfw` that try to implement a better calendar view for org mode. The experience is not as nice as google calendar though.

It can be problematic to completely give up google calendar. Shared calendars come to mind. It is hard to escape google calendar.

It's important to remember the main points of google calendars view:

-   easily find event conflicts
-   visualize the weekly schedule

Both of these points can be accomplished from the org agenda, but not as well.


# Next Steps {#next-steps}

Sometimes it's more effective to experiment then to theorize. After reflecting on the options I've decided I won't pursue the syncing strategy; dealing with syncing would produce a headache that outweights the potential benefits.

This leaves me deciding between isolated and separated. The main difference being that separated gives better weekly view, and isolated means "inline" notes for events.  At first I had an irrational fear of the separated approach, but now I am warming up to it. Even so, I'm gonna give the isolated appraoach a go for a couple weeks and see if I feel like I am missing anything.

Stay tuned by subscribing to the [rss feed](https://colekillian.com/index.xml) and feel free to leave a comment below!

Links:

-   [Irreal Post on Syncing GCal with Org Agenda](https://irreal.org/blog/?p=5839)
-   [Emacs Calendar](https://github.com/kiwanami/emacs-calfw/)
