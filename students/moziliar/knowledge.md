### GAE platform operation

Hands-on experience with managed cloud resources after deploying TEAMMATES to own staging server.

* Learnt about managing resources in GAE
* Learnt about deployment process and also scripting

### Open Source Software Challenges

Different from the (mostly) free compute resources in the enterprise, open sourced project cares about cost of every operation that's charged.
It has a totally different need from the enterprise applications.

* Learnt about real life non-profit open sourced project cost challenge
* Learnt about consideration involving cost saving in decision making
* User centric decision making

### Java

Some new Java knowledge learnt through experimentation

* Java memory reclamation by servlet is a lot faster than that within a running servlet. The direct effect is that: fetching the same data, a single servlet will result in memory spike and lingering memory residue after the call failure; whereas a series of paginated calls would result in a stable memory increment that plateaus
* Parallelism cannot be overused especially in Java where the memory is managed and GC-ed. The resources will reach contention very fast due to the generous memory allocation.
* [Link](https://stackoverflow.com/questions/11564352/arraylist-vs-linkedlist-from-memory-allocation-perspective/11564453#11564453) ArrayList apparently consumes significantly less memory than LinkedList. Choose the list implementation wisely. For non-performance critical storage, maybe ArrayList is better in all cases.

