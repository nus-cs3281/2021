### GAE platform

Hands-on experience with managed cloud resources after deploying TEAMMATES to own staging server.

* Learnt about managing resources in GAE
* Learnt about deployment process and also scripting
* Learnt about use of profiler to generate flame graph for observability
  * GAE Java 8 by default already installed profiler in the instance; just need the following in `/src/main/webapp/WEB-INF/appengine-web.xml` and apply some workload
```
    <env-variables>
        <env-var name="GAE_PROFILER_MODE" value="cpu,heap" />
    </env-variables>
```
* Learnt about changing instance class in appengine-web.xml. Refer to instance classes available [link](https://cloud.google.com/appengine/docs/standard)

GAE Related knowledge.

* Objectify 5 and prior uses Memcache by default. After GAE runtime upgrade which brings about update of Objectify 6, there will not be a default cache used. https://github.com/objectify/objectify/wiki/Caching
* Cache data decoding (deserialization) forms the main bulk of the backend memory consumption right now and, due to the managed memory of Java language, creates high memory pressure during some tasks such as feedback response result fetch. This cannot be resolved as long as we are using a cache layer before persistent storage.

### Open Source Software Challenges

Different from the (mostly) free compute resources in the enterprise, open sourced project cares about cost of every operation that's charged.
It has a totally different need from the enterprise applications.

* Learnt about real life non-profit open sourced project cost challenge
* Learnt about consideration involving cost saving in decision making
* User centric decision making
* Learnt to prioritize tasks, e.g. bugs that compromise agreement with users, over nice-to-have features

### Java

Some new Java knowledge learnt through experimentation

* Java memory reclamation by servlet is a lot faster than that within a running servlet. The direct effect is that: fetching the same data, a single servlet will result in memory spike and lingering memory residue after the call failure; whereas a series of paginated calls would result in a stable memory increment that plateaus
* Parallelism cannot be overused especially in Java where the memory is managed and GC-ed. The resources will reach contention very fast due to the generous memory allocation.
* [Link](https://stackoverflow.com/questions/11564352/arraylist-vs-linkedlist-from-memory-allocation-perspective/11564453#11564453) ArrayList apparently consumes significantly less memory than LinkedList. Choose the list implementation wisely. For non-performance critical storage, maybe ArrayList is better in all cases.
* Running a Java script (not exactly a script but a convenient class) by adding the following in the gradle file:
```
task execScript(type: JavaExec) {
    classpath = sourceSets.test.runtimeClasspath
    main = "teammates.client.scripts.YourScriptName"
}
```

