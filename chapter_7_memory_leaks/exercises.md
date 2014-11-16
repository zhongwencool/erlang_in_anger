# Exercises
## Review Questions
[1]. Name some of the common sources of leaks in Erlang programs.<br>
[2]. What are the two main types of binaries in Erlang?<br>
[3]. What could be to blame if no specific data type seems to be the source of a leak?<br>
[4]. If you find the node died with a process having a lot of memory, what could you do
to find out which one it was?<br>
[5]. How could code itself cause a leak?<br>
[6]. How can you find out if garbage collections are taking too long to run?<br>
<p></p> <font color="green">
[1]. 列出Erlang程序里面常见的泄露源。<br>
[2]. Erlang binaries有哪两种主要类型？<br>
[3]. 如果没有数据类型看上去会造成泄露，那么还应该从哪里入手？<br>
[4]. 如果你发现节点由于进程占用大量的内存而崩溃，你怎么找出来是哪一个进程？<br>
[5]. 代码本身怎么引起内存泄露？<br>
[6]. 你怎么查看到垃圾回收要花费大量的时间？<br>
</font> <p></p>

## Open-ended Questions
[1]. How could you verify if a leak is caused by forgetting to kill processes, or by processes using too much memory on their own?<br>
[2]. A process opens a 150MB log file in binary mode to go extract a piece of information
from it, and then stores that information in an ETS table. After figuring out you
have a binary memory leak, what should be done to minimize binary memory usage
on the node?<br>
[3]. What could you use to find out if ETS tables are growing too fast?<br>
[4]. What steps should you go through to find out that a node is likely suffering from
fragmentation? How could you disprove the idea that is could be due to a NIF or
driver leaking memory?<br>
[5]. How could you find out if a process with a large mailbox (from reading **message_queue_len**)?<br>
seems to be leaking data from there, or never handling new messages?<br>
[6]. A process with a large memory footprint seems to be rarely running garbage collections. What could explain this?<br>
[7]. When should you alter the allocation strategies on your nodes? Should you prefer to
tweak this, or the way you wrote code?<br>
<p></p> <font color="green">
[1].你怎么识别由于忘记杀死进程或进程自己使用太多内存而导致的泄露？<br>
[2].一个进程使用binary 模式打开了150MB的log文件，然后找到一些有用信息，然后把这些信息都存在ETS表中。在你知晓系统存在binary 内存泄露时，你应该怎样去最小化节点上的binary 内存使用。<br>
[3].你怎么发现一个ETS表内存增长过快？<br>
[4].找到一个节点是否存在内存泄露需要哪几步？你怎么验证是不是由于NIF或dirver引起的内存泄露？<br>
[5].怎么找出信箱中存在大量消息的进程(可以看下：**message_queue_len**)？<br>
&emsp;看看是内存泄露是从那里来的，还是从未处理新的消息的<br>
[6].一个有大量使用的进程看上去很少运行垃圾回收，这个要怎么解释？<br>
[7].怎么改变节点的分配器分配策略？你喜欢调整配置，还是自己写代码？<br>
</font> <p></p>

## Hands-On
[1]. Using any system you know or have to maintain in Erlang (including toy systems),
can you figure out if there are any binary memory leaks on there?<br>
<p></p> <font color="green">
[1]. 看看使用的任何Erlang系统中(包括用来实验的系统)，你可以诊断到binary内存泄露么？<br>
</font> <p></p>
