---
alias: 
title: DDIA- Chapter 1.1 Reliability 
creation date: 2021-12-05 18:54
type: note
category: []
status:
priority:
tags:
author:
rating: 
relates: 
area: 
project:
---
link:: 
tags:: 
relations:: [Designing Data-Intensive Applications](Designing%20Data-Intensive%20Applications.md)

[<- BACK TO BOOK ](Designing%20Data-Intensive%20Applications.md)
[<- Back to Chapter 1](DDIA-%20Chapter%201.%20Reliability,%20scalability,%20and%20maintainability.md)

# Chapter 1.1 Reliability

> ***reliability***  roughly means , “continuing to work correctly, even when things go wrong.”

For software, typical expectations include:

- The application performs the function that the user expected.
- It can tolerate the user making mistakes or using the software in unexpected ways.
- Its performance is good enough for the required use case, under the expected load and data volume.
- The system prevents any unauthorized access and abuse.

### Faults, Failure and Fault-tolerant systems

- The things that can go wrong are called *faults*, and systems that anticipate faults and can cope with them are called *fault-tolerant* or *resilient*. 
- 100% *fault-tolerant* software is not feasible.
- Note that a fault is not the same as a failure. A fault is usually defined as one component of the system deviating from its spec, whereas a ***failure*** is when the system as a whole stops providing the required service to the user
- it isusually best to design fault-tolerance mechanisms that prevent faults from causing failures.

### Testing Fault-tolerant systems

In fault-tolerant systems, faults are triggered deliberately to test failures before it happens in real worlds. Many critical bugs are actually due to poor error handling. By deliberately inducing faults, you ensure that the fault-tolerance machinery is continually exercised and tested. The Netflix *Chaos Monkey* is an example of this approach.



## Hardware Faults

Hard disks are reported as having a mean time to failure (MTTF) of about 10 to 50 years [[5](https://learning.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/ch01.html#Ford2010vv), [6](https://learning.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/ch01.html#Beach2014ui)]. Thus, on a storage cluster with 10,000 disks, we should expect on average one disk to die per day.

Our first response is usually to add redundancy to the individual hardware components in order to reduce the failure rate of the system.

However, as data volumes and applications’ computing demands have increased, more applications have begun using larger numbers of machines, which proportionally increases the rate of hardware faults.

Hence there is a move toward systems that can tolerate the loss of entire machines, by using software fault-tolerance techniques in preference or in addition to hardware redundancy.

Such systems also have operational advantages: system that can tolerate machine failure can be patched one node at a time, without downtime of the entire system (a *rolling upgrade*).

## Software Errors

Another class of fault is a **systematic error** within the system. Such faults are harder to anticipate, and because they are correlated across nodes, they tend to cause many more system failures than uncorrelated hardware faults. For example 

- A software bug that causes every instance of an application server to crash when given a particular bad input. 
- A runaway process that uses up some shared resource.
- A service that the system depends on that slows down, becomes unresponsive, or starts returning corrupted responses.

Usually software is making some kind of assumption about its environment—and while that assumption is usually true, it eventually stops being true for some reason.

Lot of small things can help in avoiding systematic error:

- carefully thinking about assumptions and interactions in the system.
- thorough testing
- allowing processes to crash and restart.
- measuring, monitoring, and analyzing system behavior in production

## Human Errors

 Humans are known to be unreliable.

> one study of large internet services found that configuration errors by operators were the leading cause of outages, whereas hardware faults (servers or network) played a role in only 10–25% of outages. 
>
> Recent facebook outage is also an example of this.

The best systems combine several approaches:

- Design systems in a way that minimizes opportunities for error.
- Decouple the places where people make the most mistakes from the places where they can cause failures.
- Test thoroughly at all levels, from unit tests to whole-system integration tests and manual tests
- Allow quick and easy recovery from human errors, to minimize the impact in the case of a failure. For example, make it fast to roll back configuration changes, roll out new code gradually (so that any unexpected bugs affect only a small subset of users),
- Set up detailed and clear monitoring, such as performance metrics and error rates.In other engineering disciplines this is referred to as *telemetry*. 

There are situations in which we may choose to sacrifice reliability in order to reduce development cost (e.g., when developing a prototype product for an unproven market) or operational cost (e.g., for a service with a very narrow profit margin)—but we should be very conscious of when we are cutting corners.
