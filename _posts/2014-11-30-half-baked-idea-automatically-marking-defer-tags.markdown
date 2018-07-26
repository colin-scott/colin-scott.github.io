---
layout: post
title: "Half-Baked Idea: Automatically Marking Deferred Javascript"
date: 2014-11-30 12:31:59 -0800
comments: true
categories: 
---

A typical web page is composed of multiple objects: HTML files, Javascript
files, CSS files, images, etc..

When your browser loads a web page, it executes a list of tasks:
first it needs to fetch the main HTML, then it can parse each of the
HTML tags to know what other objects to
fetch, then it can process each of the fetched objects and their effect on the
DOM, and finally it can render pixels to your screen.

To load your web page as fast as possible, the browser tries to execute as
many of these tasks as it can *in parallel*. The less time the browser
spends sitting idle waiting for tasks to finish, the faster the web page
will load.

It is not always possible to execute tasks in parallel. This is
because some tasks have dependencies on others. The most obvious example
is that the browser needs to fetch the main HTML before it can know
what other objects to fetch [1].

In general, the more dependencies a web page has,
the longer it will take to load. Prudent web developers structure their web
pages in a way that minimizes browsers' task dependencies.

A particularly nasty dependency is Javascript execution. Whenever the browser
encounters a Javascript tag, it stops all other parsing and rendering tasks, waits to fetch the
Javascript, executes it until completion, and finally restarts the previously
blocked tasks. Browsers enforce this dependency because Javascript can modify the DOM;
by modifying the DOM, Javascript might affect the execution of all other
parsing and rendering tasks.

Placing Javascript tags in the beginning of an HTML page can have a huge
performance hit, since each script adds 1 RTT plus computation time
to the overall page load time.

Fortunately, the HTML standard provides a mechanism that allows developers to mitigate this
cost: the [defer attribute](https://www.w3schools.com/tags/att_script_defer.asp). The defer attribute tells the browser
that it's OK to fetch and execute a Javascript tag asynchronously.

Unfortunately, using the defer tag is not straightforward. The issue is
that it's hard for the web developer to know whether it's safe to allow the browser to execute Javascript asynchronously.
For instance, the Javascript may actually need to modify the DOM to ensure the correct execution of the page, or it
may depend on other resources (e.g. other Javascript tags).

Forcing web developers to reason about these complicated (and often hidden!)
dependencies is, at best, a lot to ask for, and at worst, highly error-prone.
For this reason few web developers today make use of defer tags.

So here's my half-baked idea: wouldn't it be great if we had a compiler that
could automatically mark defer attributes? Specifically, let's apply static
or dynamic analysis to infer when it's safe for Javascript
tags to execute asynchronously. Such a tool could go a long way towards improving the
performance and correctness of the web.

#### Footnotes

[1] See the [WProf paper](https://www.usenix.org/system/files/conference/nsdi13/nsdi13-final177.pdf) for a nice overview of browser activity dependencies.
