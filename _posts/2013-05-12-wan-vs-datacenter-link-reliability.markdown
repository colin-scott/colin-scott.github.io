---
layout: post
title: "WAN vs. Datacenter Link Reliability"
date: 2013-05-12 15:46
comments: true
categories: 
---

According to a study by Turner et al. [1], wide area network links have an
average of 1.2 to 2.7 days of downtime per year. This translates to roughly
two and a half 9's of reliability [2].

I was curious how this compared to datacenter links, so I took a look at Gill
et. al's paper [3] on datacenter network failures at Microsoft. Unfortunately some
of the data has been redacted, but I was able to reverse engineer the mean
link downtime per year with the help of [Aurojit Panda's](https://www.eecs.berkeley.edu/~apanda/)
[svg-to-points](https://github.com/apanda/svg-points) converter. The results
are interesting: out of all links types, the average downtime was 0.3 days.
This translates to roughly three and a half 9's of reliability, an order of magnitude greater
than WAN links.

Intuitively this makes sense. WAN links are much more prone to
[drunken hunters, bulldozers, wild dogs,](https://www.cs.cornell.edu/projects/ladis2009/talks/dean-keynote-ladis2009.pdf)
[ships dropping anchor](https://www.zetatalk.com/newsletr/issue284.htm) and the like than links within a [secure](https://www.wired.com/wiredenterprise/2012/10/data-center-easter-eggs/) datacenter.

#### Footnotes

[1] Daniel Turner, Kirill Levchenko, Alex C. Snoeren, and Stefan Savage. California Fault Lines: Understanding the Causes and Impact of Network Failures, Table 4. SIGCOMM '10.

[2] Note that this statistic is specifically about hardware failure, not overall network availability.

[3] Phillipa Gill, Navendu Jain, Nachiappan Nagappan. Understanding Network Failures in Data Centers: Measurement, Analysis, and Implications, Figures 8c & 9c. SIGCOMM '11

