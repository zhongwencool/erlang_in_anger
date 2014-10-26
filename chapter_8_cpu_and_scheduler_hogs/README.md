# Chapter 8 CPU and Scheduler Hogs
While memory leaks tend to absolutely kill your system, CPU exhaustion tends to act like
a b ottleneck and limits the maximal work you can get out of a no de. Erlang develop ers
will have a tendency to scale horizontally when they face such issues. It is often an easy
enough job to scale out the more basic pieces of co de out there. Only centralized global
state (pro cess registries, ETS tables, and so on) usually need to b e mo dified. <sup>1</sup> Still, if you
want to optimize locally before scaling out at first, you need to be able to find your CPU
and scheduler hogs.
<br>&emsp;It is generally difficult to properly analyze the CPU usage of an Erlang node to pin
problems to a specific piece of code. With everything concurrent and in a virtual machine,
there is no guarantee you will find out if a specific process, driver, your own Erlang code,
NIFs you may have installed, or some third-party library is eating up all your processing
power.
<br>&emsp;The existing approaches are often limited to profiling and reduction-counting if it’s in
your code, and to monitoring the scheduler’s work if it might be anywhere else (but also
your code).


[1] Usually this takes the form of sharding or finding a state-replication scheme that’s suitable, and little
more. It’s still a decent piece of work, but nothing compared to finding out most of your program’s semantics
aren’t applicable to distributed systems given Erlang usually forces your hand there in the first place.
