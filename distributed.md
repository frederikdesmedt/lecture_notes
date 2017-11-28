Distributed Systems
==========

Feedback RMI sessions
-----

On the exam: be to-the-point and explain why.  
TODO: Look back to our submission with the slides next to it, and make sure we know what's wrong by the exam.

Replication: Fault-tolerant services
--------

Fault-tolerant part is the toughest of the course.

Goal of passive replication: **not** high availability; but to "hide faults".

A fault in the scope of distributed systems: a node that is down (a binary property).

Because of single-copy semantics the system cannot be highly available or performant.

With passive replication the primary is the main bottleneck.

**Why is there a front-end?** Replication-transparency: hiding that there is any replication.

All kinds of transparency are achieved using indirections.

*View-synchronous group communication*: a complicated communication primitive that gives us group communication, see written lecture notes for more information.

When primary node is down with passive replication we have a disruption in availability (that's why we might not have high availability).

If we expect **single-copy semantics** we expect there to be a unique ordering, which should correspond with the real-time ordering, i.e., linearizability.
We achieve this with passive replication by considering the primary node clock to be the "real-time clock", so we use that one to decide ordering.

> Active replication := all nodes do the work (they're all *active*).

With active replication the front-end receives the response from every replication node. So we can be more flexible: wait until every node answers, use the first answer we receive, use the answer most commonly received, ...

A fail-stop model: the only kind of faults are failing nodes.
Note how these are not the only kinds of faults, e.g., one of the nodes is lying to you (*cyber terrorrism*).

Default active replication policy waits for answers of all nodes, and chooses most common answer which allows us to have:

- $\frac{n}{2} - 1$ byzantine faults
- and $n - 1$ fail stops.

Passive replication cannot handle any byzantine faults and $n - 1$ fail stops.

If we would like single-copy semantics in active replication: we require totally-ordered multicast communication (so that all nodes receive all changes in the same ordering).

> Totally-ordered multicast := all messages are received in the same ordering to all nodes.

In passive replication, if we directly read data from a non-primary node, we lose linearizability (because we no longer use the "global clock", i.e., the clock of the primary).

Replication: Highly available services
-----------

Let's use both caching + replication. First file system that does this is Coda. We can get high availability with replication by not pursuing single-copy semantics (it would be nice, but it's not the primary concern). In such a network $n - 1$ nodes could go down. A cache allows us to have higher availability but obviously reduces consistency.

In the lecture, we extend AFS to implement a highly available NFS. What do we do when:

- opening a file? Go to any replication node, we do not want to go to all of them to get a consistent view, since we do not really care about consistency.
- closing a file? Send the changes that occurred to all replication nodes.

This is *read one, write all* semantics.
It provides us with a *path* to consistency ($R + W > N$, where $R$ is amount of nodes read from, $W$ amount of nodes written to, $N$ number of nodes), though not *every* path provides consistency.

*Write all* provides us with consistency, as long as nothing goes wrong (no messages get lost, no hosts go down, ...)

> **Volume storage group** (**VSG**) := a group of servers responsible for storing a volume (volume := group of files).

At some point in time, a server in a VSG can be down or disconnected. When that happens, changes send during closing of a file are not send to this server.

> **AVSG** := the group of servers **that are available** and are responsible for storing a volume (this is a subset of the VSG). It gives us a VSG for a volume *at some point in time*.

> **Coda Version Vectors** := A version counter for all the files in the group. There is a vector for each server, the vector contains $f$ components, where $f$ is the amount of files. If a change occurs the component for that file gets incremented (if the corresponding server for that vector is available).

Now, the moment they are all connected again, they can compare versions, if one has an outdated version (i.e. it's counter is lower) it can get the up-to-date version from another server.

**Coda Version Vectors** is very lightweight compared to active and passive replication, since we do not need total-ordered multicast, i.e., we have no expensive primitives.
We no longer need these complex primitives, since we no longer care about fault-tolerance.
