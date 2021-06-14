+++
title = "Binary Search Means Don't Fail The Same Way Twice"
date = 2021-06-10T22:55:00-04:00
tags = ["binary-search"]
categories = ["Thoughts"]
draft = false
+++

# Learning Fast in Gymnastics {#learning-fast-in-gymnastics}

When I did gymnastics, I spent a lot of time trying to learn new gymnastics moves. A common trick for learning these skills more quickly was to avoid failing the same way twice.

This is well demonstrated by handstands; when training handstands there are two main ways you can fail:

1.  By falling forwards onto your back
2.  By falling backwards onto your stomach

The idea is that by alternating the ways in which you are failing, you will master the skill more quickly.

-   If you are falling backwards everytime you try a handstand, you make very incremental progress.
-   If you switch between the two, you are able to more quickly get a sense of how to stay upright, neither falling forwards nor backwards.


# An Application of Binary Search {#an-application-of-binary-search}

It occured to me many years later that this is strategy is just an application of an abstracted binary search.

The main condition to perform a binary search is that the sequence must be monotonous i.e., it must be either increasing or decreasing. In the case of a handstand, the monotonous function is such that applying too much force with your wrists will cause you to fall backwards, while applying too little force will cause you to fall forwards.

It turns out that you can apply binary search to all sorts of scenarios of everyday life where you want to reach an optimum more quickly:

-   learning handstands
-   learning soccer juggling
-   getting out of your comfort zone
-   deciding how much time to study for a test

I've found that the important critera for being able to apply binary search well to real life are:

-   the existence of a function that is monotonic
-   the existence of a function whose inputs and outputs can be easily measured
-   a short and inexpensive feedback loop

Let me know if you can think of more applications in your own life!
