---
layout: post
title:  "Intro to Consensus"
date:   2020-02-10 02:25:59 -0500
categories: consensus
---

As promised, the first topic I wanted to post on is a very gentle introduction to consensus.  Of course, most everyone is aware of how the word is used in everyday language (Google tells me it is: "a general agreement"), but today we're going to discuss consensus in its distributed systems sense.  This post will be deliberately opaque about the internals of these systems, and try to keep to a very high and approachable level.

## What is consensus?

In distributed systems, we have a set of nodes all working together to accomplish some task.  And, these nodes need to coordinate with eachother to ensure that the task gets accomplished.  In this sense, distributed systems are no different than typical human day to day interactions; two parents, two children, one needs to be picked up from dance practice, the other needs to be picked up from gymnastics.  If the parents don't come to consensus, then they could both end up at the same location and one child not picked up.

For distributed systems, consensus typically means coming to an agreement on order.  Once an order has been agreed upon, coming to a consensus on other facets of the distributed is usually trivial.  To return to our parents picking up the kids example, if we say "the first parent to decide gets their pick, and the second parent picks up the kid the first one did not", then we've solved our distributed systems problem.

## What is a fault?

In a perfect world, all of the nodes (for our purposes, think computers) in your distributed system would perform their jobs perfectly.  If this were the case, designing and running distributed systems would be a fairly easily solved problem;  computer A will decide how the work is distributed and aggregate the results, computers B through Z will do what computer A tells them to.

Of course, the world is not perfect and we can't rely on all nodes in the system doing their job correctly (or on time).  There are different kinds of faults (which we'll get into in a minute), but when a node fails to perform its duties, we call this a *fault*.

Yet somehow despite these faults, the internet (which, depends on a multitude of distributed systems) and everything that we build on it seems to generally work.  So, these distributed systems must be *fault tolerant*.  But not all faults are created equal, and not all consensus algorithms tolerate all types of faults.

# Crash faults

Typically, and especially through the early 2000s, if someone told you about a 'fault tolerant distributed system' they were building, the implied sort of fault that's being referred to is a *crash fault*.  A crash fault is when a node goes offline for some reason; maybe the process crashed, maybe the network switch broke, or the datacenter admin tripped over the power cord.  In the event of a crash, we want the distributed system to continue working, and, after that datacenter admin brushes himself off and plugs the server back in, we want it to catch up and begin participating again.

# Byzantine Faults

Over the last few years, especially after the advent of blockchains, when someone advertises 'fault tolerance', they might be referring to a different sort of fault called a [*byzantine fault*](https://www.microsoft.com/en-us/research/publication/byzantine-generals-problem/), Byzantine faults are a superset of crash faults, where instead of simply not responding, or responding with out of date information, nodes can respond with arbitrary messages.  In particular, a byzantine node may respond deliberately and maliciously with bad information.  It may tell some of the network one thing, and tell another part of the network a different thing. 

## Other aspects of consensus

# Synchronous vs. Asynchronous

Today, when we think about distributed systems, we typically think about them operating on a computer network, like the internet.  But, much of the original work in consensus and distributed computing was actually around getting the internals of computers to cooperate.

Almost any modern computer (your cell phone for instance) has multiple processors, executing independently, they must ultimately coordinate with eachother.  This too is a sort of distributed system.  In these classical distributed systems, the time it took to send a message between nodes was easily bounded by the properties of the hardware, so a message would always be delivered within a set amount of time, unless a crash had occurred.  Because this amount of time was known, these consensus systems were designed with that in mind and are called synchronous.

But, synchronous consensus was thought to be impractical over internet-like networks, and so most modern consensus algorithms are asynchronous (although thanks to the [FLP impossibility](https://www.the-paper-trail.org/post/2008-08-13-a-brief-tour-of-flp-impossibility/) these algorithms ultimately rely on some synchronous assumptions).  But, much to many people's surprise, synchronous consensus algorithms like Bitcoin's proof of work have proven to be very successful operating on the asynchronous internet.

# Finality vs. Probabilistic

Classically, once consensus algorithms have established a decision, that decision is final and irrevocable.  If the algorithm declares that the order is "A then B", all nodes may safely begin to make decisions based on this fact.  However, some recent consensus algorithms (again, notably Bitcoin's proof of work), do not provide finality, and instead only give a probabilistic guarantee that the order will not change.  As the algorithm proceeds, over time, the older decisions become more and more certain not to change.  Finally, some consensus algorithms such as the TON consensus algorithm even mix the two concepts with initial probabilistic decisions and eventual finality.
