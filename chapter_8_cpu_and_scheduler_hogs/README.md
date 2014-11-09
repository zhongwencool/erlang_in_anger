# Chapter 8 CPU and Scheduler Hogs
While memory leaks tend to absolutely kill your system, CPU exhaustion tends to act like
a bottleneck and limits the maximal work you can get out of a node. Erlang developers
will have a tendency to scale horizontally when they face such issues. It is often an easy
enough job to scale out the more basic pieces of code out there. Only centralized global
state (process registries, ETS tables, and so on) usually need to be modified. <sup>1</sup> Still, if you want to optimize locally before scaling out at first, you need to be able to find your CPU and scheduler hogs.
<p></p> <font color="green">
&emsp;当内存泄露差不多要把你的系统杀死时，CPU耗尽就成为一个瓶颈，限制你可以控制节点的最大工作量。当Erlang开发者面临这个问题时，会倾向于纵向扩展(scale horizontally)。通常向外扩展基本的代码是非常简单的工作。只需要集中修改<sup>1</sup>全局状态(进程注册信息，ETS表等)。当然，如果你想在扩展前先局部优化下，你需要找出CPU和调度器的问题(hogs)。
</font> <p></p>

&emsp;It is generally difficult to properly analyze the CPU usage of an Erlang node to pin problems to a specific piece of code. With everything concurrent and in a virtual machine, there is no guarantee you will find out if a specific process, driver, your own Erlang code, NIFs you may have installed, or some third-party library is eating up all your processing power.
<br>&emsp;The existing approaches are often limited to profiling and reduction-counting if it’s in your code, and to monitoring the scheduler’s work if it might be anywhere else (but also your code).
<p></p> <font color="green">
&emsp;分析Erlang节点CPU的使用情况来找到是定位是哪一部分代码出问题通常会很难。由于一切都是在虚拟机里并行，就不能保证你会发现一个特定的进程，driver,你自己的Erlang代码,你定制的NIF，或某个第三方库耗尽了你所有的处理能力。<br>
&emsp;现有的方法往往局限于在您的代码中分析和减少计数(reduction-counting),并监控调试程序的工作，看看有没有可能会是其它地方(但还是在你的代码里)。
</font> <p></p>


[1] Usually this takes the form of sharding or finding a state-replication scheme that’s suitable, and little more. It’s still a decent piece of work, but nothing compared to finding out most of your program’s semantics aren’t applicable to distributed systems given Erlang usually forces your hand there in the first place.

<p></p> <font color="green">
[注1]：通常这需要分片的形式或找到一个合适的状态复制(state-replication)方案。这仍然是一个不错的工作，但相比于优化你程序里大部分不适合分布式系统的代码语义(sematics)来说，优化不适合于分布式的代码才是你首先需要做的。
</font> <p></p>

