# Chapter 7 Memory Leaks
# 内存泄露
There are truckloads of ways for an Erlang node to bleed memory. They go from extremely
simple to astonishingly hard to figure out (fortunately, the latter type is also rarer), and it’s possible you’ll never encounter any problem with them.<br>
&emsp;You will find out about memory leaks in two ways:<br>
<p></p>
&emsp;1. A crash dump (see Chapter 6);<br>
&emsp;2. By finding a worrisome trend in the data you are monitoring.<br>
&emsp;This chapter will mostly focus on the latter kind of leak, because they’re easier to
investigate and see grow in real time. We will focus on finding what is growing on the
node and common remediation options, handling binary leaks (they’re a special case), and
detecting memory fragmentation.
<p></p> <font color="green">
&emsp;Erlang节点内存泄露有各种千奇百怪的原因。他们有些非常容易找到原因，有些则难以找到(所幸的是，这样的情况并不多)，也有可能你永远也碰不到他们。<br>
&emsp;你可以用以下两种方法找到内存泄露：<br>
&emsp; 1. crash dump文件（查看章节6）；<br>
&emsp; 2. 通过找到你监控数据中的某些的异常项。<br>
</font> <p></p>
<p></p> <font color="green">
&emsp;本章大部分会集中在后面这种情况。因为他们在运行时很容易被看出来。我们会集中于怎么找到在节点上异常高的指标和常见的修复选项，处理二进制泄漏(他们是一个特例)，检测内存碎片。<br>
</font> <p></p>

