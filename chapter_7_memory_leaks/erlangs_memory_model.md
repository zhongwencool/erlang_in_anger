# Erlang's Memory Model
## The Global Level
To understand where memory goes, one must first understand the many allocators being
used. Erlang’s memory model, for the entire virtual machine, is hierarchical. As shown in
Figure 7.1, there are two main allocators, and a bunch of sub-allocators (numbered 1-9).
![](http://erlang-in-anger.qiniudn.com/chapter7_1.png)
<br>&emsp;Figure 7.1: Erlang’s Memory allocators and their hierarchy. Not shown is the special super carrier, optionally allowing to pre-allocate (and limit) all memory available to the Erlang VM since R16B03.

The sub-allocators are the specific allocators used directly by Erlang code and the VM for
most data types: <sup>14</sup>
<br>&emsp;1. **temp_alloc**: does temporary allocations for short use cases (such as data living within
a single C function call).
<br>&emsp;2. **eheap_alloc**: heap data, used for things such as the Erlang processes’ heaps.
<br>&emsp;3. **binary_alloc**: the allocator used for reference counted binaries (what their ’global
heap’ is). Reference counted binaries stored in an ETS table remain in this allocator.
<br>&emsp;4. **ets_alloc**: ETS tables store their data in an isolated part of memory that isn’t
garbage collected, but allocated and deallocated as long as terms are being stored in
tables.
<br>&emsp;5. **driver_alloc**: used to store driver data in particular, which doesn’t keep drivers
that generate Erlang terms from using other allocators. The driver data allocated
here contains locks/mutexes, options, Erlang ports, etc.
<br>&emsp;6. **sl_alloc**: short-lived memory blocks will be stored there, and include items such as
some of the VM’s scheduling information or small buffers used for some data types’
handling.
<br>&emsp;7. **ll_alloc**: long-lived allocations will be in there. Examples include Erlang code itself
and the atom table, which stay there.
<br>&emsp;8. **fix_alloc**: allocator used for frequently used fixed-size blocks of memory. One example of data used there is the internal processes’ C struct, used internally by the VM.
<br>&emsp;9. **std_alloc**: catch-all allocator for whatever didn’t fit the previous categories. The
process registry for named process is there<br>
![](http://erlang-in-anger.qiniudn.com/chpter_7_2.png)
<br>&emsp;By default, there will be one instance of each allocator per scheduler (and you should
have one scheduler per core), plus one instance to be used by linked-in drivers using async
threads. This ends up giving you a structure a bit like in Figure 7.1, but split it in N parts
at each leaf.
<br>&emsp;Each of these sub-allocators will request memory from **mseg_alloc** and **sys_alloc**
depending on the use case, and in two possible ways. The first way is to act as a multiblock
carrier (mbcs), which will fetch chunks of memory that will be used for many Erlang terms
at once. For each mbc, the VM will set aside a given amount of memory (about 8MB
by default in our case, which can be configured by tweaking VM options), and each term
allocated will be free to go look into the many multiblock carriers to find some decent space
in which to reside.
<br>&emsp;Whenever the item to be allocated is greater than the single block carrier threshold
(sbct) <sup>15</sup>, the allocator switches this allocation into a single block carrier (sbcs). A single
block carrier will request memory directly from **mseg_alloc** for the first **mmsbc** <sup>16</sup> entries,
and then switch over to **sys_alloc** and store the term there until it’s deallocated.
<br>&emsp;So looking at something such as the binary allocator, we may end up with something
similar to Figure 7.2
![](http://erlang-in-anger.qiniudn.com/chapter7_3.png)
<br>&emsp;&emsp;Figure 7.3: Example memory allocated in a specific sub-allocator
<br>&emsp;Whenever a multiblock carrier (or the first mmsbc <sup>17</sup> single block carriers) can be reclaimed, mseg_alloc will try to keep it in memory for a while so that the next allocation
spike that hits your VM can use pre-allocated memory rather than needing to ask the
system for more each time.
<br>&emsp;You then need to know the different memory allocation strategies of the Erlang virtual
machine:
<br>&emsp;1. Best fit (**bf**)
<br>&emsp;2. Address order best fit (**aobf**)
<br>&emsp;3. Address order first fit (**aoff**)
<br>&emsp;4. Address order first fit carrier best fit (**aoffcbf**)
<br>&emsp;5. Address order first fit carrier address order best fit (**aoffcaobf**)
<br>&emsp;6. Good fit (**gf**)
<br>&emsp;7. A fit (**af**)
<br>&emsp;Each of these strategies can be configured individually for each alloc_util allocator <sup>18</sup>
<br>&emsp;For best fit (bf), the VM builds a balanced binary tree of all the free blocks’ sizes, and
will try to find the smallest one that will accommodate the piece of data and allocate it
there. In Figure 7.3, having a piece of data that requires three blocks would likely end in area 3.
![](http://erlang-in-anger.qiniudn.com/chapter7_4.png)
<br>&emsp;Figure 7.4: Example memory allocated in a specific sub-allocator
*Address order best fit *(**aobf**) will work similarly, but the tree instead is based on the
addresses of the blocks. So the VM will look for the smallest block available that can
accommodate the data, but if many of the same size exist, it will favor picking one that
has a lower address. If I have a piece of data that requires three blocks, I’ll still likely end
up in area 3, but if I need two blocks, this strategy will favor the first mbcs in Figure 7.3
with area 1 (instead of area 5). This could make the VM have a tendency to favor the same
carriers for many allocations.
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
