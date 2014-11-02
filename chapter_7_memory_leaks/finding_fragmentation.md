# Finding Fragmentation
The **recon_alloc** module was developed specifically to detect and help point towards the
resolution of such issues.<br>
&emsp;Given how rare this type of issue has been so far over the community (or happened
without the developers knowing what it was), only broad steps to detect things are defined.
They’re all vague and require the operator’s judgement.
<p></p> <font color="green">
&emsp; **recon_alloc**模块是为检测和帮助找到这类问题的解决方案而特别开发的。<br>
&emsp;鉴于目前为止，这类问题在社区里非常罕见(或即使发生了，开发者也不知道它是什么)，所以只有定义的比较宽泛的检测步骤，具体怎么判断还是要依赖于开发者自己的判断。
</font> <p></p>

## Check Allocated Memory
Calling **recon_alloc:memory/1** will report various memory metrics with more flexibility
than **erlang:memory/0**. Here are the possibly relevant arguments:<br>
<br>&emsp;1. call** recon_alloc:memory(usage)** . This will return a value between 0 and 1 representing a percentage of memory that is being actively used by Erlang terms versus
the memory that the Erlang VM has obtained from the OS for such purposes. If the
usage is close to 100%, you likely do not have memory fragmentation issues. You’re
just using a lot of it.
<br>&emsp;2. check if **recon_alloc:memory(allocated)** matches what the OS reports. <sup>12</sup> It should
match it fairly closely if the problem is really about fragmentation or a memory leak
from Erlang terms.
That should confirm if memory seems to be fragmented or not.
<p></p> <font color="green">
&emsp;调用**recon_alloc:memory/1**来查看内存指标会比**erlang:memory/0**更加灵活。下面给出一些可能有关的参数。<br>
&emsp;1.调用** recon_alloc:memory(usage)** 。它会返回一个0~1的值，表示Erlang term与Erlang VM从操作系统得到的总内存的百分比。如果这个值接近100%,那么你可能就不会有内存碎片问题，全有的内存都处于使用状态。<br>
&emsp; 2.使用**recon_alloc:memory(allocated)**查看数据与系统的报告<sup>12</sup>是不是一致，如果真的存在内存碎片或Erlang terms内存泄露,那么这个返回值应该和系统报告的一致。这就可以确定内存是不是有碎片<br>
</font> <p></p>
## Find the Guilty Allocator
Call **recon_alloc:memory(allocated_types)** to see which type of util allocator (see Section 7.3.2) is allocating the most memory. See if one looks like an obvious culprit when you
compare the results with **erlang:memory()** .<br>
Try **recon_alloc:fragmentation(current) **. The resulting data dump will show different allocators on the node with various usage ratios. <sup>13</sup>
<br>&emsp;If you see very low ratios, check if they differ when calling **recon_alloc:fragmentation(max)** ,
which should show what the usage patterns were like under your max memory load.
<br>&emsp;If there is a big difference, you are likely having issues with memory fragmentation for a few specific allocator types following usage spikes.
<p></p> <font color="green">
&emsp;调用**recon_alloc:memory(allocated_types)**查看哪一种util分配器(详见章节7.3.2)分配的内存最多。再对比一下调用**erlang:memory()**的结果，看看像不像一个明显的原因。<br>
&emsp;试试**recon_alloc:fragmentation(current) **。返回的结果会显示节点上不同的分配器(allocators)的各项指标<sup>13</sup>。<br>
&emsp;如果你看到一些低于正常值的指标，再用**recon_alloc:fragmentation(max)**查看在负载最大时使用的是哪一种模式，比较他们有什么不同。<br>
&emsp;如果存在很大的差别，你可能就会有某个类型的分配器使用过度造成了内存碎片。
</font> <p></p>



[12] You can call **recon_alloc:set_unit(Type)** to set the values reported by recon_alloc in bytes,
kilobytes, megabytes, or gigabytes<br>
[13] More information is available at http://ferd.github.io/recon/recon_alloc.html
<p></p> <font color="green">
[注12]:你可以使用**recon_alloc:set_unit(Type)** 来设置recon_alloc返回值的类型是bytes，KB,MB，或GB<br>
[注13]：更多信息详见：http://ferd.github.io/recon/recon_alloc.html
</font> <p></p>
