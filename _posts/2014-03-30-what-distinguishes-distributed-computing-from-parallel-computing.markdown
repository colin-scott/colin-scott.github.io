---
layout: post
title: "What Distinguishes Distributed Computing From Parallel Computing?"
date: 2014-03-30 11:29
comments: true
categories: 
---

I recently came across a [statement](http://aphyr.com/posts/285-call-me-maybe-riak) in [Aphyr's](https://twitter.com/aphyr) excellent
[Jepsen](http://aphyr.com/tags/jepsen) blog series that caught my eye:

> "In a very real sense, [network] partitions are just really big windows of concurrency."

This statement seems to imply that distributed systems are "equivalent"
to parallel (single-machine) computing systems, for the following reason: partitions,
which occur in a network but don't really occur on a single chip [0], appear to be the key
distinguishing property of distributed systems. But if partitions are just a
special case of concurrency, then there shouldn't be any fundamental reasons
why algorithms for multicore computational models
(such as [PRAM](http://en.wikipedia.org/wiki/Parallel_random-access_machine)) wouldn't be perfectly suitable for solving all the
problems we might encounter in a distributed setting.
We know this to be false,
so I've been trying to puzzle out precisely what
properties of distributed computing distinguish it from parallel computing
[1].

I've been taught that distributed systems have two crucial features:

+ Asynchrony, or "absence of synchrony": messages from one process to another
  do not arrive immediately. In a fully asynchronous system, messages may be
  delayed for unbounded periods of time.
+ Partial failure: some processes in the system may fail while other processes
  continue executing.

<!--
Observe that in a loose sense, network partitions are a form of partial failure,
because from the perspective of the other nodes in the system, a partitioned node
is indistinguishable from a crashed node.
-->

Let's discuss these two properties separately.

### Asynchrony

Parallel systems also exhibit asynchrony, as long it's possible for
there to be a delay between one process sending a message [2]
and the other processes having the opportunity to read that message. Even on a single
machine, this delay might be induced by locks within the operating system kernel,
or by the cache coherence protocol implemented in hardware on a multicore chip.

With this in mind, let's return to Aphyr's statement.
What exactly did he mean by "big windows of concurrency"?
His article focuses on what happens when multiple clients write to the same
database key, so by "concurrency" I think he is referring to situations where multiple processes
might simultaneously issue writes to the same piece of state. But if you
think about it, the entire execution is a "big window of
concurrency" in this sense, regardless of whether the database replicas are partitioned.
By "big windows of concurrency" I think Aphyr was really talking about *asynchrony* (or more
precisely, periods of high message delivery delays),
since network partitions are hard to deal with precisely because the messages
between replicas aren't deliverable until after the partition is recovered:
when replicas can't coordinate, it's challenging (or impossible, if the system chooses to enforce linearizability)
for them to correctly process those concurrent writes. Amending Aphyr's statement then:

> "Network partitions are just really big windows of asynchrony."

Does this amendment resolve our quandary? Someone could
rightly point out that because partitions don't really occur within a single chip [0],
parallel systems can effectively provide guarantees on how long message
delays can last [3], whereas partitions in distributed systems may last
arbitrarily long. Some algorithms designed for parallel computers might
therefore break in a distributed setting,
but I don't think this is really the distinction we're looking
for.

### Partial Failure

Designers of distributed algorithms codify their assumptions
about the possible ways nodes can fail by specifying a 'failure model'. Failure models might describe
how many nodes can fail--for example, quorum-based algorithms assume that no more
than N/2 nodes ever fail, otherwise they cannot make progress--or they might
spell out how individual crashed nodes behave. The latter constraint forms a
hierarchy, where weaker failure models (e.g. 'fail-stop', where crashed nodes are guaranteed to never
send messages again) can be reduced to special cases of stronger models (e.g.
'Byzantine', where faulty nodes can behave arbitrarily, even possibly
mimicking the behavior of correct nodes) [4].

Throughout the Jepsen series, Aphyr tests distributed systems by (i) telling
clients to issue concurrent writes, (ii) inducing a network partition between
database replicas, and (iii) recovering the partition. Observe that Jepsen
never actually kills replicas! This failure model is actually weaker than fail-stop,
since nodes are guaranteed to eventually resume sending messages [5].
Aphyr's statement is beginning to make sense:

> "Network partitions that are followed by network recovery are just really big windows of asynchrony."

This statement is true; from the perspective of a node in the system, a network partition followed by a network recovery
is indistinguishable from a random spike in message delays, or peer nodes that
are just very slow to respond. In other words, a distributed system that
guarantees that messages will eventually be deliverable to all nodes is
equivalent to an asynchronous parallel system. But if any nodes in the
distributed system actually fail, we're no longer equivalent to a parallel
system.

### Who cares?

This discussion might sound like academic hairsplitting, but I claim that
these distinctions have practical implications.

As an example, let's imagine that you need to make a choice between shared memory
versus message passing as the communication model for the shiny new distributed
system you're designing. If you come from a parallel computing background you
would know that message passing is actually equivalent to shared memory, in
the sense that you can use a message passing abstraction to implement
shared memory, and vice versa. You might therefore conclude that you
are free to choose whichever abstraction is more convenient or performant for
your distributed system. If you jumped to this conclusion you might end up
making your system more fragile without realizing it.
Message passing is not equivalent to shared memory in distributed systems [6],
precisely because distributed systems exhibit *partial failures*;
in order to correctly implement shared memory in a distributed system it must
always be possible to coordinate with a quorum, or
otherwise be able to accurately detect which nodes have failed. Message
passing does not have this limitation.

Another takeaway from this discussion is that Jepsen is actually testing a
fairly weak failure mode. Despite Jepsen's simplicity though, Aphyr has managed to uncover problems in
an impressive number of distributed databases. If we want to uncover yet more implicit assumptions
about how our systems behave, stronger failure modes seem like an
excellent place to look.

----

[0] After I posted this blog post, Aphyr and others informed me that some of
the latest multicore chips are in fact facing partial failures between cores
due to voltage
issues. This is quite interesting, because as multicore chips grow in transistor density, the
distinction between parallel computing and distributed computing is becoming
more and more blurred: modern multicore chips face both unbounded
asynchrony (from the growing gap between levels of the memory hierarchy) and partial failure (from voltage
issues).

[1] Thanks to [Ali Ghodsi](http://www.cs.berkeley.edu/~alig/) for helping me tease out the differences between these properties.

[2] or writing to shared memory, which is essentially the same as sending a
message.

[3] See, for example, PRAM or BSP, which assume that every node can
communicate with every other node within each "round". It's trivial to solve
[hard](http://groups.csail.mit.edu/tds/papers/Lynch/pods83-flp.pdf) problems like
consensus in this world, because you can always just take a majority
vote and decide within two rounds.

[4] See Ali Ghodsi's excellent [slides](http://www.cs.berkeley.edu/~alig/cs294-91/events-links.pptx) for a taxonomy of these failure models.

[5] Note that this is not equivalent to 'crash-recovery'. Crash-recovery is
actually stronger than fail-stop, because nodes *may* recover or they may
not.

[6] Nancy Lynch, "Distributed Algorithms", Morgan Kaufmann, 1996.
