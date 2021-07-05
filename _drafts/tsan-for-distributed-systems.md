---
layout: post
title: "`tsan` for Distributed Systems"
comments: true
categories:
---

Data race detectors are super useful tools for concurrent programs. Why? Because data races are so damn hard to detect -- and even when you do confirm that something "funky" is happening in your program's executions, it may take along time to narrow down the root cause.

Not all forms of non-determinism are indicitave of a bug. At their core, data races are indicitave of non-determinism.

Research directions:
  - Program analysis (or experiments!) to determine if two concurrent messages actually causally conflict with eachother (or not)
  - Checking locking disciplines for distributed locking algorithms (Paxos, Raft)
  - When multithreaded endhosts, run tsan on each of them, but guide the message orderings to attempt to trigger data races.
  - Distributed shared memory (e.g., RDMA): let's build the same insturmentation used for single-machine shared memory data race detection into a distributed system.



Re: RDMA. RDMA is essentially a (distributed) shared memory model, with a global address space. So, a data race would be: two concurrent user-space-initiated read or write operations on a particular memory location, at least one of which is a write, and at least one of which is unguarded by synchronization. 

Two points that are admittedly a bit hazy to me:
  - Memory accesses bypass the kernel. Google is implementing a "SmartNIC" that handles RDMA operations in the NIC, which has both an ASIC for fast operations and CPUs for complicated operations. You're absolutely right, I suppose we would need to add instrumentation to that SmartNIC.
  - It's not entirely clear to me how synchronization/locking works in RDMA, I'll do some reading and get back to you!

Re: General distributed systems, this idea is admittedly half-baked. After writing the email, I realized that although shared memory is ~equivalent to message passing, tools like TSan leverage a few important properties of shared memory to avoid producing too many false positives:
  - Shared memory gives the tool fine-grained knowledge about what parts of the program's state machine (memory locations) are being written to and read from. If two concurrent operations are touching entirely different memory locations, we don't consider that indicative of a bug.
  - TSan adds instrumentation to the synchronization/locking library, so that it can know (under the assumption that the synchronization/locking library is the only mechanism used by the program for synchronization) whether reads and writes are unguarded.
 
In other words, most operations are concurrent with eachother, in both shared memory and message passing models. But TSan is able to significantly widdle down the set of concurrent operations using the above two observations to find a set of operations that are most likely indicative of a bug. 

That is not true for general message passing systems; I think we would need more domain-specific knowledge of the correctness conditions of the system in order to be able to identify concurrent messages that are most likely indicative of a bug.

http://cseweb.ucsd.edu/~savage/papers/Tocs97.pdf
https://github.com/google/sanitizers/wiki/ThreadSanitizerCppManual
