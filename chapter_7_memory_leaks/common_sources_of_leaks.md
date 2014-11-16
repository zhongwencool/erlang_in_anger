# Common Sources of Leaks
# 常见的泄露源头
Whenever someone calls for help saying "oh no, my nodes are crashing", the first step is
always to ask for data. Interesting questions to ask and pieces of data to consider are:<br>
&emsp;• Do you have a crash dump and is it complaining about memory specifically? If not,
the issue may be unrelated. If so, go dig into it, it’s full of data.<br>
&emsp;• Are the crashes cyclical? How predictable are they? What else tends to happen at
around the same time and could it be related?<br>
&emsp;• Do crashes coincide with peaks in load on your systems, or do they seem to happen
at more or less any time? Crashes that happen especially during peak times are often
due to bad overload management (see Chapter 3). Crashes that happen at any time,
even when load goes down following a peak are more likely to be actual memory
issues.<br>
<p></p> <font color="green">
&emsp;无论何时有人对你说“哦,不，我的节点崩溃了啊”,第一步总是要求他提供数据。需要考虑的有趣问题有：<br>
&emsp;• 你有crash dump文件么?它有没有指示出内存有什么异常？如果内存没有异常，这个问题可能与内存泄露不相关。如果是，那么深入分析它。里面全是数据线索。<br>
&emsp;• 这个崩溃是周期性的么？怎么去预估它们？系统在发生崩溃时还发生了什么与可能崩溃相关的事件？<br>
&emsp;• 崩溃发生在系统负载最大时么？或者看上去们任何时刻都有可能发生？发生在峰值期间的崩溃经常是由于不良的负载管理(查看章节3)。那种随时都有可能发生的Crashes(甚至可能发生在负载峰值在下降的过程中),更可能是真正的内存问题。
</font> <p></p>

&emsp;If all of this seems to point towards a memory leak, install one of the metrics libraries
mentioned in Chapter 5 and/or recon and get ready to dive in. <sup>1</sup>
&emsp;The first thing to look at in any of these cases is trends. Check for all types of memory
using **erlang:memory()** or some variant of it you have in a library or metrics system. Check
for the following points:<br>
&emsp;• Is any type of memory growing faster than others?<br>
&emsp;• Is there any type of memory that’s taking the majority of the space available?<br>
&emsp;• Is there any type of memory that never seems to go down, and always up (other than
atoms)?<br>
Many options are available depending on the type of memory that’s growing.<br>
<p></p> <font color="green">
&emsp;如果所有以上的表现都指向了内存泄露，那么安装一个在章节5中提及的指标检测库或recon,然后再准备深入它<sup>1</sup>。<br>
&emsp;首先要判断是倾向于上面哪一种情况。使用**erlang:memory()**你安装工具库里面它的变种函数来检查所有类型的内存使用的以下情况：<br>
&emsp;• 有没有一种内存类型使用比其它类型的内存增长快得多？<br>
&emsp;• 有没有一种内存占用了大部分的可用空间?<br>
&emsp;• 有没有一种内存使用量从来没有降低过，一直增长(除了原子)?<br>
根据增长的内存类型，可以提供很多可用的选项。<br>
</font> <p></p>

[1] See Chapter 4 if you need help to connect to a running node<br>
<font color="green">
[注1] 如果你想连接到一个运行中的节点上，请看章节4<br>
</font>
