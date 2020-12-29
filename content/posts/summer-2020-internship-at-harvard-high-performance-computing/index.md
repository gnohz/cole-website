+++
title = "Summer 2020 Internship at Harvard High Performance Computing"
date = 2020-12-05T00:00:00-05:00
tags = ["internship", "locust"]
categories = ["Retrospectives"]
draft = false
+++

# High Level Overview {#high-level-overview}

I intered at Harvard HPC for 12 weeks (High Performance Computing) the summer of 2020.

Due to Covid, the internship was virtual. I was working from my families home in Cambridge, MA.

My work focused on improvements to the Harvard academic cluster. I was lucky to work alongside lots of awesome and brilliant colleagues. :)


# What Did I Learn {#what-did-i-learn}


## Skills {#skills}

-   I learned a lot about performance testing software. Previously i've relied on services like Firebase Hosting which remove some responsibility for performance testing and coding scalability.
-   I got very familiar with linux monitoring tools.
-   I learned "locust", a python performance testing framework.


## Lessons {#lessons}

-   Make sure to design tests based on what information you are trying to learn from them.
-   It's not always worth it to analyze the results of performance tests that assume a higher load than you would ever expect in practice.
-   Watch out for threading problems in third party packages.


# What Did I Get Done {#what-did-i-get-done}

The technical details of my work is pretty well summarized by [this presentation](https://ruborcalor.github.io/Academic-Cluster-Presentation/) I gave at the end of the internship (I didn't record it unfortunately).

Here is an overview of the things I was most proud of accomplishing:

-   After using performance testing to identify bottlenecks in the user account creation pipeline backend, I was able to modify it and improve processing speed by 15 times (Python, Bash).
-   I coded [web server performance tests](https://gist.github.com/Ruborcalor/86300f7f1a17f75c34b10b778c83962d) which exposed unexpectedly high load averages on web server hosts, and found a way to reduce that load by 70% (Locust).
-   I built a [dashboard](https://github.com/Ruborcalor/hpc-status-app) that enables non technical users to understand the complex Harvard super computer status (React, Passenger).
-   Shared my load testing script and findings with the OSC OnDemand community: [post](https://discourse.osc.edu/t/load-testing-ood/986/6).


# Thanks {#thanks}

This experience would not have been possible without the many awesome colleagues I got to work alongside.

-   Special thanks Francesco for being such a great mentor this past summer and instilling me with a sense of confidence and responsibility. I wouldn't have grown and learned as much as I did without your constant feedback.
-   Special thanks Raminder for having me back to intern this past summer and believing in me.
