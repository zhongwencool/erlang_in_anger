# Chapter 3  Planning for Overload
# 处理超负荷
By far, the most common cause of failure I’ve encountered in real-world scenarios is due to the node running out of memory. Furthermore, it is usually related to message queues going out of bounds. <sup>1</sup><br>
There are plenty of ways to deal with this, but knowing which one to use will require a decent understanding of the system you’re working on.<br>
To oversimplify things, most of the projects I end up working on can be visualized as a very large bathroom sink. User and data input are flowing from the faucet.<br>
The Erlang system itself is the sink and the pipes, and wherever the output goes (whether it’s a database, an external API or service, and so on) is the sewer system.
<p></p> <font color="green">

&emsp;目前为止,在真实场景中,我遇到过最常见导致失败的原因：节点耗尽内存。而且，内存耗尽常常是由于消息队列超出限度。<br>
&emsp;处理内存耗尽的方案有很多，但只要求你对要处理的系统有一个深入的了解，才能选择相对好的方案。<br>

为了简单直接的描述问题，大部分我工作过的项目都可以被看成是一个巨大的浴室水池，用户和输入数据从水龙头涌出来。<br>
&emsp;Erlang 系统自身相当于水池和管道，所有的输出(数据库或外部API接口，或其它服务项等等)就是下水道。
</font> <p></p>

![](http://erlang-in-anger.qiniudn.com/chapter3_1.png)
<p></p>

When an Erlang node dies because of a queue overflowing, figuring out who to blame is crucial.<br>
Did someone put too much water in the sink? Are the sewer systems backing up?Did you just design too small a pipe?
<p></p> <font color="green">
&emsp;找出谁是消息队列溢出导致Erlang节点崩溃的罪魁祸首无疑是至关重要的。<br>
是不是有人在水槽里放入了水过多，下水道堵了？还只是设计的管道太小了？
</font> <p></p>
Determining what queue blew up is not necessarily hard. This is information that can be found in a crash dump.<br>
Finding out why it blew up is trickier. Based on the role of the process or run-time inspection, it’s possible to figure out whether causes include fast flooding, blocked processes that won’t process messages fast enough, and so on.<br>
The most difficult part is to decide how to fix it. When the sink gets clogged up by too much waste, we will usually start by trying to make the bathroom sink itself larger (the part of our program that crashed, at the edge).<br>
Then we figure out the sink’s drain is too small, and optimize that. Then we find out the pipes themselves are too narrow, and optimize that.
<p></p> <font color="green">
&emsp;你可以在crash dump文件中轻易地找到什么队列挂掉了。<br>
&emsp;但是比较棘手的是为什么这队列会挂掉。这取决于进程担任的角色或运行时检查的状态(run-time inspection)，可能的原因：大量的消息涌入或进程堵塞导致不能快速地处理消息等等。<br>
&emsp;最难的部分是决定如何去修复它，当水池被大量的垃圾堵塞时，通常我们首先会试图把水池扩张得更大。<br>
&emsp;然后我们发现水池的排水也太小了，然后又优化了下排水量。紧接着又冒出问题是管道自身太窄了，马上又优化了下管道。
</font> <p></p>

The overload gets pushed further down the system, until the sewers can’t take it anymore. At that point, we may try to add sinks or add bathrooms to help with the global input level.<br>
Then there’s a point where things can’t be improved anymore at the bathroom’s level.
There are too many logs sent around, there’s a bottleneck on databases that need the consistency, or there’s simply not enough knowledge or manpower in your organization to improve things there.
<p></p> <font color="green">
&emsp;超负荷终就会慢慢降低系统的性能，直到下水道也不能承载这些负荷而崩溃。这时我们可能会通过增加水池数量来分担负荷。<br>
&emsp;然后当在水池层面已优化到最优时，
你又发现还有大量飞来飞去的日志，待克服瓶颈的数据库，又或者没有足够的知识或者人力来改善这些。
</font> <p></p>

By finding that point, we identified what the true bottleneck of the system was, and all the prior optimization was nice (and likely expensive), but it was more or less in vain.<br>
We need to be more clever, and so things are moved back up a level. We try to massage the information going in the system to make it either lighter (whether it is through compression, better algorithms and data representation, caching, and so on).<br>
Even then, there are times where the overload will be too much, and we have to make the hard decisions between restricting the input to the system, discarding it, or accepting that the system will reduce its quality of service up to the point it will crash.<br>
These mechanisms fall into two broad strategies: back-pressure and load-shedding.<br>
We’ll explore them in this chapter, along with common events that end up causing Erlang systems to blow up.
<p></p> <font color="green">

&emsp;通过以上的排查，我们终于定位到系统的瓶颈是在哪里，之前所有的优化都很好(也可能代价很大)，但这或多或少是徒劳无益的。<br>
&emsp;我们可以尝试用更加明智的方法，先把一切都恢复原样。我们可以尝试把系统朝更轻量来优化(不管是使用压缩，更优的算法，或更好的数据结构，缓存等等).<br>

&emsp;即使这样，有些情况的负荷还是会很大，那么我们就不得不选择重构或放弃系统的输入系统，或接受系统达到这种程度后就会崩溃的设定。<br>
&emsp;这些机制可以分为两大策略:back-pressure和load-shedding.
我们可以浏览本章节，了解到那些导致Erlang崩溃的常见事件.
</font> <p></p>

[1] Figuring out that a message queue is the problem is explained in Chapter 6, specifically in Section 6.2
<p></p> <font color="green">

[注1]：你可以在章节6的6.2找到消息队列的相关问题描述。
</font> <p></p>
