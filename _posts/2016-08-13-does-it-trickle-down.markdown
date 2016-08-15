---
layout: post
title: "Do the benefits of technological innovation trickle down?"
bibliography: bib.bib
date: 2016-08-13 00:01:12
comments: true
categories:
---

In my job interviews earlier this year I had the opportunity to speak with many interesting people.
I posed the following question to several of my interviewers:

> Do technological innovations [that computer scientists produce] benefit society as
a whole, or do they primarily improve the livelihood of people and institutions who are already well off?

Strikingly, I received a nearly identical response from three people I spoke to. Their response was
something along the lines of the following:

>  "The innovations from computer science (are one of the few types of innovation that)
    trickle down"

In this blog post I seek to evaluate their claim.

### Scoping the hypothesis

The question I posed was admittedly vague. What exactly did I mean by
`benefit', and how can we possibly make a statement about
all technological innovations?

To scope the discussion, let's focus on a more specific question:

>  Does progress in information technology correlate with an improvement in poverty rates?

We'd like to know, at a macroscopic level, whether IT helps people get out of poverty. Causation
is difficult to argue, but if there is causation we should expect there to
also be correlation.[^1]

### Data from the United States

In the following figure, Kentaro Toyama
[examines](https://www.youtube.com/watch?v=cxutDM2r534) the
data for the United States, using the federal
government's [definition of
poverty](https://en.wikipedia.org/wiki/Poverty_in_the_United_States):

![US Poverty Data](http://eecs.berkeley.edu/~rcs/research/kentaro_data.png){:height="800px" width="800px"}

As we see, poverty rates in the USA haven't budged since the early 1970s, and
the *absolute* number of people living in poverty has actually gone up. This
is despite huge advances in the proliferation of information technologies like
the Internet, the world wide web, and smartphones.

From this, we can reasonably conclude that information technology *alone* has not had much
of an effect on poverty numbers in the USA; there must be some other factors
preventing such a large number of people from getting out of poverty.

### Data from the World

The USA is just one country, and it is an anomalous country in many ways. Perhaps
information technology does more to help the impoverished in the rest of the world?
The remarkable chart below uses the World Bank's definition[^2]
for extreme poverty at $1.25 per day adjusted to purchasing power parity:

![Worldwide Poverty Data](http://eecs.berkeley.edu/~rcs/research/poverty_global.jpg){:height="800px" width="800px"}

According to one reading, this data does not provide reason to doubt our
trickle down hypothesis:
there is a downward trend in the absolute number of people living in
extreme poverty, which may be partially due to the proliferation of information
technologies.

A less optimistic
[reading](http://www.humanosphere.org/basics/2015/09/global-poverty-falling-not-fast-may-think/) is that the decline in poverty
numbers we see starting in the 1970s (well before the popularity of the Web) can largely be attributed to
changes in the Chinese government's polices, and, more recently, India's own reforms starting in 1991. It seems entirely feasible that
information technology played a negligible role in these transformations.

Personally, I'm not convinced that the innovations we (systems researchers) produce, which are often
targeted towards the more privileged members of society, currently play a significant role in
the socioeconomic well-being of the poor.

### Caveats, caveats, caveats

My conclusion certainly does not imply that computer scientists should stop
focusing on innovations targeted at the more privileged members of society.
It's very difficult to predict what kinds of impact our innovations will have; for example,
the Apple engineers who developed the iPhone were explicitly targeting the
rich, yet the popularity of the iPhone sparked a drive towards cellular data
networks which are now the dominant mode of Internet in the developing world.

It's also undeniably true that information technology positively touches the lives of the poor,
even if the jury is still out on whether it can play a significant role in helping significant numbers of them develop their
socioeconomic well-being.

Personally, I've decided to pivot my direction away from high tech. I'm
devoting the next year or two to understanding what role, if any, IT can play in the lives of the less privileged. We'll
see what I learn!

---

[^1]: Strictly speaking, neither correlation nor lack thereof prove anything about causation. But most scientists probably think [Hume](https://en.wikipedia.org/wiki/Problem_of_induction) was just being pedantic.

[^2]: The World Bank announced plans to move their [definition](https://en.wikipedia.org/wiki/Extreme_poverty#cite_note-7) of extreme poverty to $1.90 per day to recognize higher price levels in developing countries than previously estimated.
