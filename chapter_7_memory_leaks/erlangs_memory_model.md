# Erlang's Memory Model
## The Global Level
To understand where memory goes, one must first understand the many allocators being
used. Erlang’s memory model, for the entire virtual machine, is hierarchical. As shown in
Figure 7.1, there are two main allocators, and a bunch of sub-allocators (numbered 1-9).
<p></p> <font color="green">
&emsp;为了更好地理解内存用到什么地方了，我们必须首先认识一下使用中的多种分配器(allocators)。对于整个虚拟机来说，Erlang内存模型(memory model)是分层的。正如图7.1所示，有两种主要的分配器，一群子分配器(1~9)。
</font> <p></p>

![](http://erlang-in-anger.qiniudn.com/chapter7_1.png)
<br>&emsp;Figure 7.1: Erlang’s Memory allocators and their hierarchy. Not shown is the special super carrier, optionally allowing to pre-allocate (and limit) all memory available to the Erlang VM since R16B03.
<p></p> <font color="green">
图7.1 Erlang内存分配器及分层。没有显示的：特殊的超级航母(special super carrier),从R16B03版本后，就可以通过这个special super carrier选择性地预先分配(和限制)所有Erlang虚拟机使用的内存。
</font> <p></p>


The sub-allocators are the specific allocators used directly by Erlang code and the VM for
most data types: <sup>14</sup>
<br>&emsp;1. **temp_alloc**: does temporary allocations for short use cases (such as data living within a single C function call).
<br>&emsp;2. **eheap_alloc**: heap data, used for things such as the Erlang processes’ heaps.
<br>&emsp;3. **binary_alloc**: the allocator used for reference counted binaries (what their ’global
heap’ is). Reference counted binaries stored in an ETS table remain in this allocator.
<br>&emsp;4. **ets_alloc**: ETS tables store their data in an isolated part of memory that isn’t garbage collected, but allocated and deallocated as long as terms are being stored in tables.
<br>&emsp;5. **driver_alloc**: used to store driver data in particular, which doesn’t keep drivers that generate Erlang terms from using other allocators. The driver data allocated here contains locks/mutexes, options, Erlang ports, etc.
<br>&emsp;6. **sl_alloc**: short-lived memory blocks will be stored there, and include items such as some of the VM’s scheduling information or small buffers used for some data types’ handling.
<br>&emsp;7. **ll_alloc**: long-lived allocations will be in there. Examples include Erlang code itself and the atom table, which stay there.
<br>&emsp;8. **fix_alloc**: allocator used for frequently used fixed-size blocks of memory. One example of data used there is the internal processes’ C struct, used internally by the VM.
<br>&emsp;9. **std_alloc**: catch-all allocator for whatever didn’t fit the previous categories. The process registry for named process is there<br>
<p></p> <font color="green">
&emsp;那些子分配器(sup-allocators)是Erlang代码和大多数VM数据类型所使用的特殊分配器<sup>14</sup>：<br>
&emsp;1. **temp_alloc**:为一些短暂的使用场景(比如为使用单个C函数调用的数据）做临时的分配工作。<br>
&emsp;2. **eheap_alloc**:堆数据(heap data)，用于Erlang进程的堆分配。<br>
&emsp;3.  **binary_alloc**:用于引用记数的binary(reference counted binaries).引用记数的binaries被这个分配器存在一个ETS表中。<br>
&emsp;4. **ets_alloc**:ETS表把数据存在一个不会被垃圾回收掉的独立内存块中，但只要terms存在表中，就会触发分配与重新分配(allocated and deallocated)。<br>
&emsp;5.**driver_alloc**:专用于存放driver数据，呆以保证drivers不会使用其它类型的分配器生成Erlang terms。分配在这里的driver数据可以是locks/mutexes, options, Erlang ports等。<br>
&emsp;6.**sl_alloc**: 存放短暂存活(short-live))的内存块，包括一些VM调度信息或一些数据类型处理的小缓冲区数据。<br>
&emsp;7. **ll_alloc**: 存放长时间(long-lived)存活的数据。比如Eralng代码和原子表。<br>
&emsp;8. **fix_alloc**:使用频繁，用于分配固定大小内存块的分配器。比如进程内部的C结构，只会被VM内部使用。<br>
&emsp;9. **std_alloc**:包括不符合以上类别的所有类型的分配器。进程注册数据就放在这里。
</font> <p></p>

![](http://erlang-in-anger.qiniudn.com/chpter_7_2.png)
<br>&emsp;By default, there will be one instance of each allocator per scheduler (and you should have one scheduler per core), plus one instance to be used by linked-in drivers using async threads. This ends up giving you a structure a bit like in Figure 7.1, but split it in N parts at each leaf.
<br>&emsp;Each of these sub-allocators will request memory from **mseg_alloc** and **sys_alloc**
depending on the use case, and in two possible ways. The first way is to act as a multiblock
carrier (mbcs), which will fetch chunks of memory that will be used for many Erlang terms
at once. For each mbc, the VM will set aside a given amount of memory (about 8MB
by default in our case, which can be configured by tweaking VM options), and each term
allocated will be free to go look into the many multiblock carriers to find some decent space in which to reside.
<br>&emsp;Whenever the item to be allocated is greater than the single block carrier threshold (sbct) <sup>15</sup>, the allocator switches this allocation into a single block carrier (sbcs). A single block carrier will request memory directly from **mseg_alloc** for the first **mmsbc** <sup>16</sup> entries, and then switch over to **sys_alloc** and store the term there until it’s deallocated.
<br>&emsp;So looking at something such as the binary allocator, we may end up with something similar to Figure 7.2
<p></p> <font color="green">
&emsp;默认情况下，对于每一个调度器(每个core都会不一个调度器)都会对应一个分配器的实例，另外每个实例都会被dirvers用异步线程的方式linked-in。最终的结构有点像图7.1中给出的，但会在每个叶子(leaf)处理得了分裂成N个部分。<br>
&emsp;每个子分配器都会使用**mseg_alloc** 或 **sys_alloc**请求内存，使用哪一种就要看具体的使用场景。第一种方式就是作为一个multiblock载体(mbcs),会为很多的Erlang terms一次申请多个内存块。对于每一个mbc，VM虚拟会分配一定数量的内存(默认在我们这里大约8MB，可以通过调整VM选项来设置)，第一个term都可以自由地查看多个mutiblock载体来找到一些不错的空间驻留。<br>
&emsp;每当被分配的空间大于单个载体块(single block carrier)阈值(sbct)<sup>15</sup>，那彼分配器就会把这个分配转成一个single block carrier(sbcs)。single block carrier 首先会为第一个**mmsbc** <sup>16</sup>记录，直接通过**mseg_alloc**申请内存，然后再转给**sys_alloc**，让它把term一直存在那里直到被重新分配。<br>
&emsp;那么再看看binary的分配器，我们可能最终得到是是与图7.2类似的结果。
</font> <p></p>

![](http://erlang-in-anger.qiniudn.com/chapter7_3.png)
<br>&emsp;&emsp;Figure 7.3: Example memory allocated in a specific sub-allocator
<p></p> <font color="green">
&emsp;&emsp;图7.3：一个典型的内存子分配器
</font> <p></p>
<br>&emsp;Whenever a multiblock carrier (or the first mmsbc <sup>17</sup> single block carriers) can be reclaimed, mseg_alloc will try to keep it in memory for a while so that the next allocation spike that hits your VM can use pre-allocated memory rather than needing to ask the system for more each time.
<br>&emsp;You then need to know the different memory allocation strategies of the Erlang virtual machine:
<br>&emsp;1. Best fit (**bf**)
<br>&emsp;2. Address order best fit (**aobf**)
<br>&emsp;3. Address order first fit (**aoff**)
<br>&emsp;4. Address order first fit carrier best fit (**aoffcbf**)
<br>&emsp;5. Address order first fit carrier address order best fit (**aoffcaobf**)
<br>&emsp;6. Good fit (**gf**)
<br>&emsp;7. A fit (**af**)
<br>&emsp;Each of these strategies can be configured individually for each alloc_util allocator <sup>18</sup>

<p></p> <font color="green">
&emsp;每当一个multiblock carrier(或第一个使用mmsbc<sup>17</sup>的single block carriers)可以回收时，mseg_allock会尝试把它在内存留一小会，以便让下一次的分配时可以使用VM预先可以分配的内存，而不需要每次都向系统请求。<br>
&emsp;接下来你需要知道不同的Erlang虚拟机的内存分配策略:
<br>&emsp;1. best fit (**bf**)
<br>&emsp;2. Address order best fit (**aobf**)
<br>&emsp;3. Address order first fit (**aoff**)
<br>&emsp;4. Address order first fit carrier best fit (**aoffcbf**)
<br>&emsp;5. Address order first fit carrier address order best fit (**aoffcaobf**)
<br>&emsp;6. Good fit (**gf**)
<br>&emsp;7. A fit (**af**)
<br>&emsp;这些策略都可以为每一个alloc_util<sup>18</sup>分配器单独配置。
</font> <p></p>

![](http://erlang-in-anger.qiniudn.com/chapter7_4.png)
<br>&emsp;Figure 7.4: Example memory allocated in a specific sub-allocator<br>
<p></p> <font color="green">
&emsp;&emsp;图7.4:子分配器的典型内存分配示例
</font> <p></p>
<br>&emsp;For best fit (bf), the VM builds a balanced binary tree of all the free blocks’ sizes, and will try to find the smallest one that will accommodate the piece of data and allocate it there. In Figure 7.3, having a piece of data that requires three blocks would likely end in area 3.
<p></p> <font color="green">
&emsp; 对于best fit (bf)来说，虚拟机会为所有的自由块建立一个平衡binary树，然后尝试找到最小的一个来容纳数据块，并分配给它。在图7.3中，就有数据需要区域3中的3个块才可能装得下。
</font> <p></p>
&emsp;*Address order best fit *(**aobf**) will work similarly, but the tree instead is based on the addresses of the blocks. So the VM will look for the smallest block available that can accommodate the data, but if many of the same size exist, it will favor picking one that has a lower address. If I have a piece of data that requires three blocks, I’ll still likely end up in area 3, but if I need two blocks, this strategy will favor the first mbcs in Figure 7.3 with area 1 (instead of area 5). This could make the VM have a tendency to favor the same carriers for many allocations.
<p></p> <font color="green">
&emsp;*Address order best fit *(**aobf**)工作也类似best fit，但这颗树是基于数据块的地址。所以VM会先找可以容纳数据的最小可用数据块，但如果有很多一样的块存在，它会优先选择地址小的。如果我有一个需要3个块的数据，我依然想存在区域3中，但我需要2个块，那么这个策略就会在表7.3中区域1(代替区域5)的第一个mbcs中
</font> <p></p>


<br>&emsp;*Address order first fit* (**aoff**) will favor the address order for its search, and as soon as a
block fits, aoff uses it. Where **aobf** and **bf** would both have picked area 3 to allocate four
blocks in Figure 7.3, this one will get area 2 as a first priority given its address is lowest.
In Figure 7.4, if we were to allocate four blocks, we’d favor block 1 to block 3 because its
address is lower, whereas **bf** would have picked either 3 or 4, and **aobf** would have picked
3.
<br>&emsp;*Address order first fit carrier best fit* (**aoffcbf**) is a strategy that will first favor a carrier
that can accommodate the size and then look for the best fit within that one. So if we were
to allocate two blocks in Figure 7.4, **bf** and **aobf** would both favor block 5, aoff would
pick block 1. aoffcbf would pick area 2, because the first mbcs can accommodate it fine,
and area 2 fits it better than area 1.
<br>&emsp;*Address order first fit carrier address order best fit *(**aoffcaobf**) will be similar to
**aoffcbf**, but if multiple areas within a carrier have the same size, it will favor the one
with the smallest address between the two rather than leaving it unspecified.
<br>&emsp;*Good fit* (**gf**) is a different kind of allocator; it will try to work like best fit (**bf**), but
will only search for a limited amount of time. If it doesn’t find a perfect fit there and then,it will pick the best one encountered so far. The value is configurable through the mbsd <sup>19</sup>
VM argument.
<br>&emsp;*A fit* (**af**), finally, is an allocator behaviour for temporary data that looks for a single
existing memory block, and if the data can fit, af uses it. If the data can’t fit, af allocates
a new one.
<br>&emsp;Each of these strategies can be applied individually to every kind of allocator, so that
the heap allocator and the binary allocator do not necessarily share the same strategy.
<br>&emsp;Finally, starting with Erlang version 17.0, each alloc_util allocator on each scheduler
has what is called a mbcs pool. The mbcs pool is a feature used to fight against memory
fragmentation on the VM. When an allocator gets to have one of its multiblock carriers
become mostly empty, <sup>20</sup> the carrier becomes abandoned.
<br>&emsp;This abandoned carrier will stop being used for new allocations, until new multiblock
carriers start being required. When this happens, the carrier will be fetched from the mbcs
pool. This can be done across multiple alloc_util allocators of the same type across
schedulers. This allows to cache mostly-empty carriers without forcing deallocation of their
memory. <sup>21</sup> It also enables the migration of carriers across schedulers when they contain
little data, according to their needs.
## The Process Level
On a smaller scale, for each Erlang process, the layout still is a bit different. It basically
has this piece of memory that can be imagined as one box:<br>
`1 [                                           ]`<br>
<br>&emsp;On one end you have the heap, and on the other, you have the stack:<br>
`1 [heap | | stack]`<br>
<br>&emsp;In practice there’s more data (you have an old heap and a new heap, for generational
GC, and also a virtual binary heap, to account for the space of reference-counted binaries
on a specific sub-allocator not used by the process — **binary_alloc** vs. **eheap_alloc**):<br>
`1 [heap || stack]`<br>
<br>&emsp;The space is allocated more and more up until either the stack or the heap can’t fit in
anymore. This triggers a minor GC. The minor GC moves the data that can be kept into
the old heap. It then collects the rest, and may end up reallocating more space.
<br>&emsp;After a given number of minor GCs and/or reallocations, a full-sweep GC is performed,
which inspects both the new and old heaps, frees up more space, and so on. When a
process dies, both the stack and heap are taken out at once. reference-counted binaries are
decreased, and if the counter is at 0, they vanish.
<br>&emsp;When that happens, over 80% of the time, the only thing that happens is that the
memory is marked as available in the sub-allocator and can be taken back by new processes
or other ones that may need to be resized. Only after having this memory unused — and
the multiblock carrier unused also — is it returned to mseg_alloc or sys_alloc, which
may or may not keep it for a while longer.



[14] The complete list of where each data type lives can be found in erts/emulator/beam/erl_alloc.types<br>
[15] http://erlang.org/doc/man/erts_alloc.html#M_sbct<br>
[16] http://erlang.org/doc/man/erts_alloc.html#M_mmsbc <br>
[17] http://erlang.org/doc/man/erts_alloc.html#M_mmsbc<br>
[18] http://erlang.org/doc/man/erts_alloc.html#M_as<br>
[19] http://www.erlang.org/doc/man/erts_alloc.html#M_mbsd<br>
[20] The threshold is configurable through http://www.erlang.org/doc/man/erts_alloc.html#M_acul<br>
[21] In cases this consumes too much memory, the feature can be disabled with the options +MBacul 0.<br>
