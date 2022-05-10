# Totally Ordered Broadcast and Multicast Algorithms: A Comprehensive Survey

# Table of contents
* [Fundamentals](#Fundamentals)
    * [Analysis of Algorithms](#Analysis-of-Algorithms)

* [Setup](#setup)



Total order multicast algorithms constitute an important class of problems in distributed systems, especially in the context of fault-tolerance. In short, the problem of total order multicast consists in sending
messages to a set of processes, in such a way that all messages are delivered by all correct destinations in the same order.

# Introduction
Distributed systems and applications are notoriously difficult to build. This is mostly due to the unavoidable
concurrency of such systems, combined with the difficulty of providing a global control. This difficulty is
greatly reduced by relying on communication primitives that provide higher guarantees than standard point-to-point communication. One such primitive is called total order multicast.1 Informally, total order multicast is a
primitive that sends messages to a set of destinations and enforces a total order on the delivery of those messages
(i.e., all messages are delivered in the same order).

Total order multicast is an important primitive as it plays,
for instance, a central role when implementing the state machine approach (also called active replication) [114,
113, 104]). It has also other applications such as clock synchronization [111], computer supported cooperative
writing [7], distributed shared memory, or distributed locking [78]. More recently, it has also been shown that
an adequate use of total order multicast can significantly improve the performance of replicated databases

# System Models and Definitions

## Basic System Models
Distributed systems are modeled **as a set of processes** X = {p1....pn} that interact by exchanging messages
through communication channels. There exist a quantity of models that more or less restrict the behavior of
these systems components. When one describes a system model, the important characteristics to consider are
the synchrony and the failure mode.

### Synchrony
### Failure mode

## Oracles
### Physical Clocks
### Failure Detectors
### Coin Flip

## Agreement Problems
Agreement problems constitute a fundamental class of problems in the context of distributed systems. There
exist many different agreement problems that share a common pattern: processes have to reach some common decision, the nature of which depends on the problem. In this paper, we mostly consider four important
agreement problems: **Consensus**, **Byzantine Agreement**, **Reliable Broadcast**, and **Atomic Broadcast**.

### Consensus
Informally, the problem of Consensus is defined as follows. Every process pi begins by proposing a value vi .
Then, all processes must eventually agree on the same decision value v, which must be one of the proposed
values.

### Byzantine Agreement
The problem of Byzantine Agreement is also commonly known as the “Byzantine generals problem” [80]. In
this problem, every process has an a priori knowledge that a particular process s is supposed to broadcast a
single message m. Informally, the problem requires that all correct processes deliver the same message, which
must be m if the sender s is correct.

### Reliable Broadcast
As the name indicates, Reliable Broadcast is defined as a broadcast primitive. In short, Reliable Broadcast is a
broadcast that guarantees that each broadcasted message m is delivered by all correct processes if the process
sender (m) is correct. If sender (m) is not correct, then m must be delivered either by all correct processes or
by none of them.

### Atomic Broadcast
The problem of Atomic Broadcast is also itself an agreement problem. In short, this problem is defined as a
Reliable Broadcast problem that must also ensure that all delivered messages are delivered by all processes in
the same order.

### Important Theoretical Resutls
There are at least four fundamental theoretical results that are directly relevant to the problem of Atomic Broadcast and Consensus. First, Atomic Broadcast and Consensus are equivalent problems [47, 23], that is, if there
exists an algorithm that solves one problem, then it can be transformed to solve the other problem. Second,
there is no (deterministic) solution to the problem of Consensus in asynchronous systems if just a single process
can crash [53]. Nevertheless, Consensus can be solved in asynchronous systems extended with failure detectors [23], and in some partially synchronous systems [47, 50].

## A Note on Asynchronous Total Order Multicast Algorithms
Indeed, Total Order Multicast is not solvable in the asynchronous system model if even one process can crash.
Or put differently, it is not possible, in asynchronous systems with process failures, to guarantee both safety and liveness.

## Process Controlled Crash
Process controlled crash is the ability given to processes to kill other processes or to commit suicide. In other
words, this is the ability to artificially force the crash of a process. Allowing process controlled crash in a system
model increases its power. Indeed, this makes it possible to transform severe failures (e.g., omission, Byzantine)
into less severe failures (e.g., crash), and to emulate an “almost perfect” failure detector. However, this power
does not come without a price, and process controlled crash should be avoided whenever possible.

## Group Membership Service
A group membership service is a distributed service that is responsible for managing groups of processes. It
often used as a basic building block for implementing complex group communication systems as it allows to keep track of the membership of each process group.

In a group membership service, processes are managed according to three basic operations. Processes can
join or leave a group during the execution of the system, and processes that are suspected to have crashed are
automatically excluded from the group. The composition of process groups can thus change dynamically, and
processes are informed of these changes when they receive the view change notifications.

# Specification of Total Order Multicast