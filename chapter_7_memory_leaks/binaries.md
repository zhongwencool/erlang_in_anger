# Binaries
Erlang’s binaries are of two main types: ProcBins and Refc binaries <sup>8</sup>. Binaries up to 64 bytes are allocated directly on the process’s heap, and their entire life cycle is spent in there. Binaries bigger than that get allocated in a global heap for binaries only, and each process to use one holds a local reference to it in its local heap. These binaries are referencecounted, and the deallocation will occur only once all references are garbage-collected from
all processes that pointed to a specific binary.
<br>&emsp;In 99% of the cases, this mechanism works entirely fine. In some cases, however, the process will either:<br>
&emsp;1. do too little work to warrant allocations and garbage collection;
<br>&emsp;2. eventually grow a large stack or heap with various data structures, collect them, then get to work with a lot of refc binaries. Filling the heap again with binaries (even though a virtual heap is used to account for the refc binaries’ real size) may take a
lot of time, giving long delays between garbage collections.
<p></p> <font color="green">
&emsp;Erlang的binaries有两种主要的类型：ProBins和Refc binaries<sup>8</sup>。Binaries大于64 字节的都会被直接分配到进程的堆中，他们的一生都将呆在那里。比那更大的binaries会被分配到专用于存binaries的全局堆里面，每个使用它的进程都有一个对应它的本地引用存在自己的本地堆中。这些binaries是引用计数的，会在所有关于它的本地引用都被垃圾回收后真正释放掉。<br>
</font> <p></p>
<p></p> <font color="green">
&emsp;在99%的情景下，这个机制都会正常工作。但在下面这些情况下，就变得不一定啦：<br>
&emsp;1. 对于保证分配和垃圾回收，做的工作太少。<br>
&emsp;2. 最终生成了一个有各种数据结构的大堆栈，集中他们，然后与大量的refc binaries混合在一起工作。再次用binaries填充满堆(即使只是一个用于映射refc binaries的虚拟堆)，这可能会消耗大量时间，造成垃圾回收被延迟。
</font> <p></p>


[8] http://www.erlang.org/doc/efficiency_guide/binaryhandling.html#id65798

<p></p> <font color="green">
[注8] http://www.erlang.org/doc/efficiency_guide/binaryhandling.html#id65798
</font> <p></p>
