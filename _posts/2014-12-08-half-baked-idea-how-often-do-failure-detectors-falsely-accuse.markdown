---
layout: post
title: "Half-baked idea: Is asynchrony really that bad? Or: how often do failure detectors falsely accuse?"
date: 2014-12-08 17:32:28 -0800
comments: true
categories:
---

Distributed systems have two distinguishing features:

- Asynchrony, or "absence of synchrony": messages from one process to another
  do not arrive immediately. In a fully asynchronous system, messages may be
  delayed for unbounded periods of time. In contrast, synchronous
  networks always provide bounded message delays.
- Partial failure: some processes in the system may fail while other processes
  continue executing.

It's the combination of these two features that make distributed systems
really hard; the crux of many impossibility proofs is
that nodes in a fully asynchronous system can't distinguish message
delays from failures.

In practice, networks are somewhere between fully asynchronous and
synchronous. That is, most (but not all!) of the time, networks give us sufficiently predictable message
delays to allow nodes to coordinate successfully in the face of failures.

When designing a distributed algorithm however, common wisdom says that you should try to
make as few assumptions about the network as possible. The motivation for this
principle is that minimizing your algorithm's assumptions about message delays maximizes the likelihood that it will work when placed
in a real network (which may, in practice, fail to meet bounds on message delays).

On the other hand, if your network does in fact provide bounds
on message delays, you can often design simpler and more performant
algorithms on top of it. An example of this observation that I find particularly
compelling is [Speculative
Paxos](https://syslab.cs.washington.edu/research/specpaxos/index.html), which
co-designs a consensus algorithm and the underlying network to improve overall
performance.

At the risk of making unsubstantiated generalizations, I get the sense that
theorists (who have dominated the field of distributed computing until somewhat recently) tend
to worry a lot about corner cases that jeopardize correctness properties. That
is, it's the theorist who's telling us to minimize our assumptions.
In contrast, practitioners are often willing to sacrifice correctness in favor
of simplicity and performance, as long as the corner cases that cause the
system to violate correctness are sufficiently rare.

To resolve the tension between the theorists' and the practitioners' principles, my half-baked idea is that we
should attempt to answer the following question:
"How asynchronous are our networks in practice"?

Before outlining how one might answer this question, I need to provide a bit
of background.

### Failure Detectors

In reaction to the overly pessimistic asynchrony assumptions made by impossibility
proofs, theorists spent about a decade [1] developing distributed algorithms for "partially synchronous" network models.
The key property of the partially synchronous model is that at some point in the execution of the distributed system, the
network will start to provide bounds on message delays, but the algorithm
won't know when that point occurs.

The problem with the partial asynchrony model is that algorithms built on top
of it (and their corresponding correctness proofs) are messy: the timing assumptions of the algorithm are strewn throughout
the code, and proving the algorithm correct requires you to pull those timing
assumptions through the entire proof until you can finally check at the end
whether they match up with the network model.

To make reasoning about asynchrony easier, a theorist named Sam Toueg along
with a few others at Cornell proposed the concept of [failure detectors](https://www.cs.cornell.edu/home/sam/FDpapers/CT96-JACM.ps).
Failure detectors allow algorithms to encapsulate timing assumptions:
instead of manually setting timers to detect failures, we design our
algorithms to ask an oracle about the presence of failures [2]. To implement the oracle, we
[still](https://research.microsoft.com/en-us/people/weic/wdag97_hb.pdf) use timers,
but now we have all of our timing assumptions collected cleanly in one place.

Failure detectors form a hierarchy. The strongest failure detector has perfect
accuracy (it never falsely accuses nodes of failing) and perfect completeness
(it always informs all nodes of all failures). Weaker failure detectors might
make mistakes, either by falsely accusing nodes of having crashed, or by
neglecting to detect some failures. The different failure detectors
correspond to different points on the asynchrony spectrum: perfect failure
detectors can only be implemented in a fully synchronous network [3], whereas
imperfect failure detectors correspond to partial synchrony.

### Measuring Asynchrony

One way to get a handle on our question is to measure the behavior of failure detectors in practice.
That is, one could implement imperfect failure detectors,
place them in networks of different kinds, and measure how often they falsely
accuse nodes of failing. If we have ground truth on when nodes actually fail
in a controlled experiment, we can quantify how often those corner cases theorists
are worried about come up.

Anyone interested in getting their hands dirty?

#### Footnotes

[1] Starting in [1988](https://groups.csail.mit.edu/tds/papers/Lynch/jacm88.pdf) and dwindling after [1996](https://www.cs.cornell.edu/home/sam/FDpapers/CT96-JACM.ps).

[2] Side note: failures detectors aren't widely used in practice. Instead, most
distributed systems use ad-hoc network timeouts strewn throughout the code. At best, distributed systems use
adaptive timers, again strewn throughout the code. A library or language
that encourages programmers to encapsulate
timing assumptions and explicitly handle failure detection information could go a long
way towards improving the simplicity, amenability to automated tools, and robustness
of distributed systems.

[3] Which is equivalent to saying that they can't be implemented. Unless you
can ensure that the network itself never suffers from any failures or
congestion, you can't guarantee perfect synchrony. Nonetheless, some of the most recent [network designs](https://www.usenix.org/sites/default/files/conference/protected-files/vliu_nsdi13_slides.pdf) get us
pretty close.
