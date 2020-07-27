---
title: "How Much Do Echo Statements Slow Down a Bash Script?"
date: 2020-07-27T14:45:02-04:00
draft: false
tags: ["Bash", "Optimization", "Benchmark"]
categories: ["Technical"]
---

# Goal

I was trying to optimize a bash script and wondered how much echo statements would slow it down, so I did a few tests. I was specifically wondering if adding a check for verbosity before an echo statement would have a significant affect on runtime, vs just removing the echo statement all together.

# Script

I used this script to run the benchmark:
{{< highlight bash >}}

export TIMEFORMAT=%R	
verbose=1	
echo=foo	
echo=01234567890123456789012345678901234567890123456789	
for i in {1..10}; do	
	time (for i in {1..1000000}; do [[ verbose -eq 1 ]] && echo $echo; done > /tmp/file.log) 2>> foocheckTrue.log
	time (for i in {1..1000000}; do [[ verbose -eq 0 ]] && echo $echo; done > /tmp/file.log) 2>> foocheckFalse.log
	time (for i in {1..1000000}; do echo $echo; done > /tmp/file.log) 2>> foonoCheck.log
	time (for i in {1..1000000}; do : ; done) 2>> foonoPrint.log
done	
{{</ highlight >}}

I set `echo` to `01234567890123456789012345678901234567890123456789` when benchmarking for long echo statements, and to `foo` when benchmarking short echo statements.

# Results

## Long Echo Statement


| Trial   | Don't check for verbosity and don't print | Check for verbosity and don't print | Don't check for verbosity and print | Check for verbosity and print |
|---------|-------------------------------------------|-------------------------------------|-------------------------------------|-------------------------------|
|       1 |                                     1.505 |                               2.232 |                               4.048 |                         5.642 |
|       2 |                                     1.461 |                               2.258 |                               4.056 |                         5.496 |
|       3 |                                     1.531 |                                2.27 |                               4.166 |                         5.481 |
|       4 |                                     1.531 |                                2.31 |                               4.187 |                         5.508 |
|       4 |                                     1.522 |                                4.11 |                               4.188 |                         7.195 |
|       5 |                                      1.83 |                               3.532 |                                5.29 |                         6.008 |
|       6 |                                       1.9 |                               3.262 |                               6.116 |                         6.696 |
|       7 |                                     1.795 |                               2.669 |                               4.901 |                         7.356 |
|       8 |                                     1.835 |                               2.454 |                               4.163 |                         6.418 |
|       9 |                                     1.536 |                               2.946 |                               4.555 |                         6.636 |
| Average |                                    1.6446 |                              2.8043 |                               4.567 |                        6.2436 |

## Short Echo Statement

| Trial   | Don't check for verbosity and don't print | Check for verbosity and don't print | Don't check for verbosity and print | Check for verbosity and print |
|---------|-------------------------------------------|-------------------------------------|-------------------------------------|-------------------------------|
|       1 |                                     1.471 |                               2.229 |                               2.909 |                         4.464 |
|       2 |                                     1.481 |                               2.233 |                               3.001 |                         4.602 |
|       3 |                                     1.496 |                               2.236 |                                2.94 |                         4.433 |
|       4 |                                     1.473 |                               2.226 |                               2.994 |                         4.682 |
|       4 |                                     1.477 |                               2.275 |                               2.892 |                         4.368 |
|       5 |                                     1.537 |                               2.333 |                               3.275 |                         4.547 |
|       6 |                                     1.559 |                               2.244 |                               2.931 |                         4.493 |
|       7 |                                     1.507 |                               2.184 |                               2.961 |                         4.475 |
|       8 |                                     1.468 |                               2.185 |                               2.864 |                          4.37 |
|       9 |                                     1.483 |                               2.204 |                               2.864 |                         4.357 |
| Average |                                    1.4952 |                              2.2349 |                              2.9631 |                        4.4791 |

# Conclusion

The different options had significant differences in runtime. Checking for verbosity and not printing took an average of `1.5 - 1.7` times as long as not printing anything at all! I definitely took this into consideration when deciding which echo statements were really necessary for my bash script.

It's worth noting that this benchmark is based on `echoing` to a file. By echoing to the console the options that print would take significantly longer.

Let me know if you have any comments below!
