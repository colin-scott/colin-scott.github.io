---
layout: post
title: "`tsan` for Distributed Systems"
comments: true
categories:
---

Data race detectors are super useful tools for concurrent programs. Why? 

Not all forms of non-determinism are indicitave of a bug. At their core, data races are indicitave of non-determinism.

Research directions:
  - Program analysis (or experiments!) to determine if two concurrent messages actually causally conflict with eachother (or not)
  - Checking locking disciplines for distributed locking algorithms (Paxos, Raft)
  - When multithreaded endhosts, run tsan on each of them, but guide the message orderings to attempt to trigger data races.

http://cseweb.ucsd.edu/~savage/papers/Tocs97.pdf
https://github.com/google/sanitizers/wiki/ThreadSanitizerCppManual
