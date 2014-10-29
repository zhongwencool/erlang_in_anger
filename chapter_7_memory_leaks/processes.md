# Processes
There are a lot of different ways in which process memory can grow. Most interesting
cases will be related to a few common cases: process leaks (as in, you’re leaking processes),
specific processes leaking their memory, and so on. It’s possible there’s more than one
cause, so multiple metrics are worth investigating. Note that the process count itself is
skipped and has been covered before.
<p></p> <font color="green">
进程内存增长的方法也有各种各样的途径。大部分有意思的原因都是以下这几种情况引起的：进程泄露(就和字面意思一样，你在泄露进程),特定的进程内存泄漏等。可能不只一个原因，所以你可以多个指标一起分析。注意，我们现在跳过了进程数的分析，因为前面已讲过啦。
</font> <p></p>

## Links and Monitors
Is the global process count indicative of a leak? If so, you may need to investigate unlinked processes, or peek inside supervisors’ children lists to see what may be weird-looking.<br>
&emsp;Finding unlinked (and unmonitored) processes is easy to do with a few basic commands:<br>
<p></p> <font color="green">
&emsp;总的进程数是不是暗示着泄露？如果是，那么你需要调查下那些unlinked的进程，或仔细看看supervisors的子列表是不是有什么奇怪的东西混进来。<br>
&emsp;使用下面的命令可以非常容易就能找到unlinked(或unmonitored)的进程：<br>
</font> <p></p>

-----------------------------------------------------<br>
`1> [P || P <- processes(),`<br>
`[{_,Ls},{_,Ms}] <- [process_info(P, [links,monitors])],`<br>
`[]==Ls, []==Ms].`<br>
-----------------------------------------------------<br>
This will return a list of processes with neither. For supervisors, just fetching
supervisor: count_children(SupervisorPid Or Name) and seeing what looks normal can
be a good pointer.<br>
<p></p> <font color="green">
&emsp;它会返回没有link和monitor的进程列表。对于supervisors来说，只需取得supervisor的子进程的数量(SupervisorPid或Name),查下这个子进程数是不是正常会很有帮助。
</font> <p></p>

## Memory Used
The per-process memory model is briefly described in Subsection 7.3.2, but generally speaking, you can find which individual processes use the most memory by looking for their
memory attribute. You can look things up either as absolute terms or as a sliding window.
<br>&emsp;For memory leaks, unless you’re in a predictable fast increase, absolute values are usually those worth digging into first:<br>
<p></p> <font color="green">
&emsp;对于每个进程的的内存模型在章节7.3.2已简明描述过了，但通常来讲，你可以通过查看内存属性得到使用内存最多的那个进程。你可以通过绝对项目(absolute terms)或滑动视窗(sliding window)来查看。
&emsp;对于内存泄露，首先深挖绝对值(absolute values)对找到问题非常有帮助，除非你处理一个可以预测的快速增长阶段。<br>
</font> <p></p>

-----------------------------------------------------<br>
`1> recon:proc_count(memory, 3).`<br>
`[{<0.175.0>,325276504,`<br>
`[myapp_stats,`<br>
`{current_function,{gen_server,loop,6}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.169.0>,73521608,`<br>
`[myapp_giant_sup,`<br>
`{current_function,{gen_server,loop,6}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.72.0>,4193496,`<br>
`[gproc,`<br>
`{current_function,{gen_server,loop,6}},`<br>
`{initial_call,{proc_lib,init_p,5}}]}]`<br>
-----------------------------------------------------<br>
&emsp;Attributes that may be interesting to check other than memory may be any other fields in Subsection 5.2.1, including **message_queue_len**, but memory will usually encompass all other types.
<p></p> <font color="green">
&emsp;内存的其它有意思的属性可以在章节5.2.1看到，比如**message_queue_len**,但内存通常会涵盖其它类型。
</font> <p></p>

## Garbage Collections
It is very well possible that a process uses lots of memory, but only for short periods of time.
For long-lived nodes with a large overhead for operations, this is usually not a problem, but
whenever memory starts being scarce, such spiky behaviour might be something you want
to get rid of.<br>
Monitoring all garbage collections in real-time from the shell would be costly. Instead,
setting up Erlang’s system monitor <sup>7</sup> might be the best way to go at it.
Erlang’s system monitor will allow you to track information such as long garbage collection periods and large process heaps, among other things. A monitor can temporarily
be set up as follows:<br>
<p></p> <font color="green">
&emsp;一个进程只在一个非常短的时间内使用了大量内存的情况也是非常有可能发生的。对于一个处于大负荷操作的长期运行的节点来说，这种情况通常不会引起问题，但是当内存增长到难以统计时(非常大)，这种突然的行为就有可能是你想要摆脱的东西。<br>
&emsp;在shell里面实时监测所有垃圾回收情况是代价是非常大的。取而代之，设置Erlang系统本身的monitor<sup>7</sup>可能最处理它的最好方式。Erlang系统监控会允许你追踪长周期的垃圾回收和大进程的堆情况，此外，监控可以用以下方式来临时设置：
</font> <p></p>

-----------------------------------------------------<br>
`1> erlang:system_monitor().`<br>
`undefined`<br>
`2> erlang:system_monitor(self(), [{long_gc, 500}]).`<br>
`undefined`<br>
`3> flush().`<br>
`Shell got {monitor,<4683.31798.0>,long_gc,`<br>
`[{timeout,515},`<br>
`{old_heap_block_size,0},`<br>
`{heap_block_size,75113},`<br>
`{mbuf_size,0},`<br>
`{stack_size,19},`<br>
`{old_heap_size,0},`<br>
`{heap_size,33878}]}`<br>
`5> erlang:system_monitor(undefined).`<br>
`{<0.26706.4961>,[{long_gc,500}]}`<br>
`6> erlang:system_monitor().`<br>
`undefined`<br>
-----------------------------------------------------<br>
&emsp;The first command checks that nothing (or nobody else) is using a system monitor yet — you don’t want to take this away from an existing application or coworker.
<br>&emsp;The second command will be notified every time a garbage collection takes over 500 milliseconds. The result is flushed in the third command. Feel free to also check for
**{large_heap, NumWords}** if you want to monitor such sizes. Be careful to start with large
values at first if you’re unsure. You don’t want to flood your process’ mailbox with a bunch
of heaps that are 1-word large or more, for example.
<br>&emsp;Command 5 unsets the system monitor (exiting or killing the monitor process also frees
it up), and command 6 validates that everything worked.
<p></p> <font color="green">
&emsp;第一个命令作用：检查下sysytem_monitor有没有被其它使用--你不能从一个已存在的application或合作进程中把它抢过来。<br>
&emsp;第二个命令作用：设置垃圾回收每500ms就通知一下shell，shell得到结果要通过第三个命令刷新一下才能看到。如果你想检测大小，那么可以随时检查**{large_heap, NumWords}**。如果你不确定，那么就不要在开始时就监测lager values。你肯定也不想你的信箱被大量的大于1-word的堆撑爆吧。<br>
&emsp;第五个命令作用：释放system monitory(退出或杀掉你的进程也可以释放),第六个命令作用：验证下一切工作正常。
</font> <p></p>

<br>&emsp;You can then find out if such monitoring messages tend to coincide with the memory increases that seem to result in leaks or overuses, and try to catch culprits before things are too bad. Quickly reacting and digging into the process (possibly with recon:info/1)
may help find out what’s wrong with the application.
<p></p> <font color="green">
&emsp;通过这样监视消息，找到内存增长原因往往是内存泄漏或过度使用，试图在一切变得糟糕之前就找出罪犯。快速地反应并深入到进程当中(可能会使用到recon:info/1)可能会有助于找到application出了出了什么问题。
</font> <p></p>


[7] http://www.erlang.org/doc/man/erlang.html#system_monitor-2<br>

<p></p> <font color="green">
[注7] http://www.erlang.org/doc/man/erlang.html#system_monitor-2<br>
</font> <p></p>
