# General View
Reading the crash dump will be useful to figure out possible reasons for a node to die a posteriori. One way to get a quick look at things is to use recon’s **erl_crashdump_analyzer.sh** <sup>4</sup>
and run it on a crash dump:<br>
------------------------------------------------------------<br>
`$ ./recon/script/erl_crashdump_analyzer.sh erl_crash.dump`<br>
`analyzing erl_crash.dump, generated on: Thu Apr 17 18:34:53 2014`<br>
`Slogan: eheap_alloc: Cannot allocate 2733560184 bytes of memory`<br>
`(of type "old_heap").`<br>
`Memory:`<br>
`===`<br>
`processes: 2912 Mb`<br>
`processes_u sed: 2 912 Mb`<br>
`system: 8167 Mb`<br>
`atom: 0 Mb`<br>
`atom_used: 0 Mb`<br>
`binary: 3243 Mb`<br>
`code: 11 Mb`<br>
`ets: 4755 Mb`<br>
`---`<br>
`total: 11079 Mb`<br>
`Different message queue lengths (5 largest differen t):`<br>
`===`<br>
`1 5010932`<br>
`2 159`<br>
`5 158`<br>
`49 157`<br>
`4 156`<br>
`Error logger queue length:`<br>
`===`<br>
`0`<br>
`File descriptors open:`<br>
`===`<br>
`UDP: 0`<br>
`TCP: 19951`<br>
`Files: 2`<br>
`---`<br>
`Total: 19953`<br>
`Number of processes:`<br>
`===`<br>
`36496`<br>
`Processes Heap+Sta ck memo ry siz es (wor ds) us ed in the VM (5 largest`<br>
`different):`<br>
`===`<br>
`1 284745853`<br>
`1 5157867`<br>
`1 4298223`<br>
`2 196650`<br>
`12 121536`<br>
`Processes OldHeap memory sizes (words) used in the VM (5 largest`<br>
`different):`<br>
`===`<br>
`3 318187`<br>
`9 196650`<br>
`14 121536`<br>
`64 75113`<br>
`15 46422`<br>
`Process States when crashing (sum):`<br>
`===`<br>
`1 Garbing`<br>
`74 Scheduled`<br>
`36421 Waiting`<br>
------------------------------------------------------------<br>
This data dump won’t point out a problem directly to your face, but will be a good clue
as to where to look. For example, the node here ran out of memory and had 11079 Mb out
of 15 Gb used (I know this because that’s the max instance size we were using!) This can
be a symptom of:<br>
&emsp;• memory fragmentation;<br>
&emsp;• memory leaks in C code or drivers;<br>
&emsp;• lots of memory that got to be garbage-collected before generating the crash dump <sup>5</sup>.<br>
&emsp;More generally, look for anything surprising for memory there. Correlate it with the
number of processes and the size of mailboxes. One may explain the other.<br>
&emsp;In this particular dump, one process had 5 million messages in its mailbox. That’s
telling. Either it doesn’t match on all it can get, or it is getting overloaded. There are
also dozens of processes with hundreds of messages queued up — this can point towards
overload or contention. It’s hard to have general advice for your generic crash dump, but
there still are a few pointers to help figure things out.<br>

[4] https://github.com/ferd/recon/blob/master/script/erl_crashdump_analyzer.sh<br>
[5] Notably here is reference-counted binary memory, which sits in a global heap, but ends up being garbage-collected before generating the crash dump. The binary memory can therefore be underreported. See Chapter 7 for more details
