# Can’t Allocate Memory
These are by far the most common types of crashes you are likely to see. There’s so much to
cover, that Chapter 7 is dedicated to understanding them and doing the required debugging
on live systems.<br>
&emsp;In any case, the crash dump will help figure out what the problem was after the fact.
The process mailboxes and individual heaps are usually good indicators of issues. If you’re
running out of memory without any mailbox being outrageously large, look at the processes
heap and stack sizes as returned by the recon script.<br>
&emsp;In case of large outliers at the top, you know some restricted set of processes may be
eating up most of your node’s memory. In case they’re all more or less equal, see if the
amount of memory reported sounds like a lot.<br>
&emsp;If it looks more or less reasonable, head towards the "Memory" section of the dump
and check if a type (ETS or Binary, for example) seems to be fairly large. They may point
towards resource leaks you hadn’t expected.<br>
<p></p> <font color="green">
&emsp;以上是迄今为止你所能见到的最常见的crashes。这涵盖了太多的内容，章节7致力于在需要调试的系统上理解他们。<br>
&emsp;无论如何，crash dump文件都有助于找出事实问题背后的原因是什么。进程信箱和个人堆(individual heaps，进程私有堆空间)通常是一个很好的线索。如果你在没有信箱异常多消息时就耗尽了内存，可以试试用recon脚本返回的进程的堆栈大小。<br>
</font> <p></p>
<p></p> <font color="green">
&emsp;由于一些很大的异常值，会有一些严格的进程限制（some restricted set of processes）可能会吃掉你大部分的节点内存。在这种情况下，进程之间基本都是平等的(每个进程都吃掉一点，总量变大很多)，导致报告中的内存总量看上去很多。<br>
&emsp;如果上面所说这这些都是合理的，再检查下在dump里面关于"Memory"的部分，看看是否有东西(ETS或二进制))看上去非常大。他们可能会引起意想不到的资源泄露。<br>
</font> <p></p>

