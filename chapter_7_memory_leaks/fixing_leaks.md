# Fixing Leaks
Once you’ve established you’ve got a binary memory leak using **recon:bin_leak(Max)** , it
should be simple enough to look at the top processes and see what they are and what kind
of work they do.
<br>&emsp;Generally, refc binaries memory leaks can be solved in a few different ways, depending
on the source:
<br>&emsp;• call garbage collection manually at given intervals (icky, but somewhat efficient);
<br>&emsp;• stop using binaries (often not desirable);
<br>&emsp;• use** binary:copy/1-2**<sup>10</sup> if keeping only a small fragment (usually less than 64 bytes)
of a larger binary;<sup>11</sup>
<br>&emsp;• move work that involves larger binaries to temporary one-off processes that will die
when they’re done (a lesser form of manual GC!);
<br>&emsp;• or add hibernation calls when appropriate (possibly the cleanest solution for inactive
processes).
<br>&emsp;The first two options are frankly not agreeable and should not be attempted before all
else failed. The last three options are usually the best ones to be used.
<p></p> <font color="green">
&emsp;一旦你使用**recon:bin_leak(Max)**确定了你有binary 内部泄露问题，找出前几名的进程并查看他们是什么，负责的是什么工作是非常简单的一件事。
</font> <p></p>

<p></p> <font color="green">
&emsp;通常，refc binaries内存泄露可能通过以下几种方式解决（这取决于源代码实现）：<br>
&emsp;• 指定周期内手动调用垃圾回收。(很讨厌，但是效果显著)<br>
&emsp;• 停止使用binaries(经常不能随你意的，你不可能不使用binaries);<br>
&emsp;• 使用**binary:copy1-2**<sup>10</sup> 如果只是操作一小块<sup>11</sup>的binary的话(常常小于64bytes);<br>
&emsp;• 把涉及到大块binaries的操作都放到短暂的一次性进程中，这个进程完成任务就会死掉触发GC(一种较小的手动GC);<br>
&emsp;• 或者在适当的时候调用休眠(hibernation)(对于不活跃的进程来讲可能是最彻底的方案)。<br>
&emsp;坦白来说，前2种方案是不能优先使用的，它应该在尝试了其它的方案后失败后才考虑。后3个方案通常是最好用的方案。
</font> <p></p>

## Routing Binaries
There’s a specific solution for a specific use case some Erlang users have reported. The
problematic use case is usually having a middleman process routing binaries from one
process to another one. That middleman process will therefore acquire a reference to every
binary passing through it and risks being a common major source of refc binaries leaks.
The solution to this pattern is to have the router process return the pid to route to and
let the original caller move the binary around. This will make it so that only processes that
do need to touch the binaries will do so.
<br>&emsp;A fix for this can be implemented transparently in the router’s API functions, without
any visible change required by the callers.
<p></p> <font color="green">
&emsp;Erlang用户报告里面对于一些具体的情景给出了特定的方案。有问题的使用方式通常是用一个中间进程把binaries从一个进程路由到另一个进程。那么这个中间进程就需要对每个路由过的binary都有一个引用。这就是最常见的refc binaries泄露的源头。这种模式的解决方法就是路由进程只保存pid，并让最原始的调用进程来移动binary。这就使得只有需要接触binaries的进程才拥有binaries的引用。<br>
&emsp;修复这个可以把路由API函数的实现透明化，对于调用者来说没有任何明显的变化。
</font> <p></p>

[10] http://www.erlang.org/doc/man/binary.html#copy-1
[11] It might be worth copying even a larger fragment of a refc binary. For example, copying 10 megabytes
off a 2 gigabytes binary should be worth the short-term overhead if it allows the 2 gigabytes binary to be
garbage-collected while keeping the smaller fragment longer.

<p></p> <font color="green">
[注10]： http://www.erlang.org/doc/man/binary.html#copy-1<br>
[注11]: 有时，复制大的碎片式的refc binary是值得去做的。比如，从2GB的binary中复制出10MB这种短期的开销就非常值得，这样可以允许2GB的binary被垃圾回收，但同时保存着那10MB的碎片binary。
</font> <p></p>



