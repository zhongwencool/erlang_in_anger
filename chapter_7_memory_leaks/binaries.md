# Binaries
Erlang’s binaries are of two main types: ProcBins and Refc binaries <sup>8</sup>. Binaries up to 64
bytes are allocated directly on the process’s heap, and their entire life cycle is spent in
there. Binaries bigger than that get allocated in a global heap for binaries only, and each
process to use one holds a local reference to it in its local heap. These binaries are referencecounted, and the deallocation will occur only once all references are garbage-collected from
all processes that pointed to a specific binary.
<br>&emsp;In 99% of the cases, this mechanism works entirely fine. In some cases, however, the process will either:<br>
<br>&emsp;1. do too little work to warrant allocations and garbage collection;
<br>&emsp;2. eventually grow a large stack or heap with various data structures, collect them, then get to work with a lot of refc binaries. Filling the heap again with binaries (even though a virtual heap is used to account for the refc binaries’ real size) may take a
lot of time, giving long delays between garbage collections.


[8] http://www.erlang.org/doc/efficiency_guide/binaryhandling.html#id65798
