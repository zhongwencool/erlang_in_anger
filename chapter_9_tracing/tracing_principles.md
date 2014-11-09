# Tracing Principles
The Erlang Trace BIFs allow to trace any Erlang code at all <sup>8</sup>. They work in two parts: *pid specifications*, and *trace patterns*.
<br>&emsp;**Pid specifications** lets the user decide which processes to target. They can be specific pids, all pids, **existing** pids, or **new** pids (those not spawned at the time of the function call).
<br>&emsp;The **trace patterns** represent functions. Functions can be specified in two parts: specifying the modules, functions, and arity, and then with Erlang match specifications <sup>9</sup> to add constraints to arguments.
<br>&emsp;What defines whether a specific function call gets traced or not is the intersection of both, as seen in Figure 9.1.
If either the **pid specification** excludes a process or a** trace pattern** excludes a given call, no trace will be received.
<br>&emsp;Tools like **dbg** (and trace BIFs) force you to work with this Venn diagram in mind.
You specify sets of matching pids and sets of trace patterns independently, and whatever
happens to be at the intersection of both sets gets to be displayed.
<br>&emsp;Tools like **redbug** and **recon_trace**, on the other hand, abstract this away.
<p></p> <font color="green">
&emsp;Erlang的Trace BIFs允许trace任意的Erlang代码 <sup>8</sup>。他们工作分2部分：*pid specifications*, and *trace patterns*。<br>
&emsp;**Pid specifications**让使用者决定那些进程是目标。他们可以是**specified pids**，**all pids**，**existing pids**，或**new pids**（那些不是在函数调用时间内派生出来的)。
</font> <p></p>

<p></p> <font color="green">
&emsp;**trace patterns**代表函数，函数可以分成两类：指定模块，函数，和参数和使用Erlang匹配规格<sup>9</sup>添加约束参数。<br>
&emsp;一个特定的函数调用是否被trace决定于两者的交集，如下图9.1所示。如果一个进程即不包括 **pid specification**，也不包括** trace pattern**，那么就不会收到trace消息。<br>
&emsp;**dbg** (各 trace BIFs)的工具就必须要你把这个图强记于心。<br>
&emsp;像**redbug** 和 **recon_trace**的工具，则简化了这种方式。
</font> <p></p>

![](http://erlang-in-anger.qiniudn.com/chapter9_1.png)
<br>&emsp;Figure 9.1: What gets traced is the result of the intersection between the matching pids and the matching trace patterns
<p></p> <font color="green">
&emsp;&emsp;图9.1可以得到trace结果的情况：matching pids和matching trace patterns之间
</font> <p></p>

[8] In cases where processes contain sensitive information, data can be forced to be kept private by calling process_flag(sensitive, true)<br>
[9] http://www.erlang.org/doc/apps/erts/match_spec.html<br>

<p></p> <font color="green">
[注8]：在这种情况下,过程包含敏感信息，数据可调用process_flag(sensitive, true)强制保存为私有数据。<br>
[注9]：http://www.erlang.org/doc/apps/erts/match_spec.html<br>
</font> <p></p>

