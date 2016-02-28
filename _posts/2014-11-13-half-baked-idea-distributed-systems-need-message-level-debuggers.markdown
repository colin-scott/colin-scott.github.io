---
layout: post
title: "Half-Baked Idea: Distributed Systems Need Message-Level Debuggers"
date: 2014-11-13 19:26:12 -0800
comments: true
categories: 
---

gdb, although an incredibly powerful tool for debugging single programs, doesn't work
so well for distributed systems.

The crucial difference between a single program and a distributed system is that distributed
computation revolves around *network messages*. Distributed systems spend
much of their time doing nothing more than waiting for network messages. When
they receive a message, they perform computation, perhaps send out a few
network messages of their own, and then return to their default state of
waiting for more network messages.

Because there's a network separating the nodes of the distributed system, you
can't (easily) pause all processes and attach gdb. And, in the words of
[Armon Dadgar](https://twitter.com/armon/status/533050582995435520),
"even if you could, the network is part of your system. Definitely not going to be able to gdb
attach to that."

Suppose that you decide to attach gdb to a single process in the distributed
system. Even then, you'll probably end up frustrated. You're going to spend most
of your time waiting on a `select` or `receive` breakpoint. And when your
breakpoint is triggered, you'll find that most of the
messages won't be relevant for triggering *your* bug. You need to
wait for a specific message, or even a specific sequence of messages, before
you'll be able to trace through the code path that leads to your bug.

Crucially, gdb doesn't give you the ability to control network
messages, yet network message are what drive the distributed
system's execution. In other words, gdb operates at a level of abstraction that is lower than what you
want.

Distributed systems need a different kind of debugger. What we need is a
debugger that will allow us to step through the distributed system's
execution at the level of network messages. That is, you should be able to
generate messages, control the order in which they arrive, and observe how the
distributed system reacts.

Shameless self-promotion: [STS](http://ucb-sts.github.io/sts/walkthrough#interactive_mode)
supports an "Interactive Mode" that takes over control of the
(software) network separating the nodes of a distributed system. This allows
you to interactively reorder or drop messages, inject failures, or check
invariants. We need something like this for testing and debugging general distributed systems.
