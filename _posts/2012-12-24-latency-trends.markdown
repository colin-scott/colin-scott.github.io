---
layout: post
title: "Latency Trends"
date: 2012-12-24 15:20
comments: true
categories: 
---

In 2010, Jeff Dean gave a [talk](http://goo.gl/0MznW) that laid out
a list of [numbers](https://gist.github.com/2843375) every programmer
should know. His list has since become relatively well known among the systems community.

The other day, a friend mentioned a latency number to me, and I realized that
it was an order of magnitude smaller than what I had memorized from
Jeff's talk. The problem, of course, is that hardware performance increases
exponentially! After some digging, I actually found that the numbers Jeff
quotes are over a decade old [1].

Partly inspired by my officemate [Aurojit Panda](http://www.eecs.berkeley.edu/~apanda/), who is collecting
awesome [data](http://www.eecs.berkeley.edu/~rcs/research/hw_trends.xlsx) on
hardware performance, I decided to write a little tool [2] to visualize Jeff's
numbers as a function of time [3].

Without further ado,
[here](http://www.eecs.berkeley.edu/~rcs/research/interactive_latency.html) it
is.

#### Footnotes

[1] Jeff's numbers are from 2001, and were first publicized by Peter Norvig in this
[article](http://norvig.com/21-days.html#answers).

[2] Layout stolen directly from [ayshen](https://github.com/ayshen) on GitHub.

[3] The hardware trends I've gathered are rough estimates. If you want to tweak
the parameters yourself, I've made it really easy to do so -- please send me
updates! Better yet, issue a [pull request](https://github.com/colin-scott/interactive_latencies).
