# Memory Fragmentation
Memory fragmentation issues are intimately related to Erlang’s memory model, as described
in Section 7.3.2. It is by far one of the trickiest issues of running long-lived Erlang nodes
(often when individual node uptime reaches many months), and will show up relatively
rarely.
<br>&emsp;The general symptoms of memory fragmentation are large amounts of memory being allocated during peak load, and that memory not going away after the fact. The
damning factor will be that the node will internally report much lower usage (through
**erlang:memory()** ) than what is reported by the operating system.
<p></p> <font color="green">
&emsp;正如章节7.3.2所说，内存碎片问题与Erlang的内存模型(memory model)有密切的关系。对于长时间运行的Erlang节点来说，它是迄今为止最棘手的问题之一(通常个别节点会连续运行达数月之久)，不过它出现的机会相对较少。<br>
&emsp;内存碎片常见的症状就是在最大负载时做大量的内存分配，事后内存却没有被释放掉。更要命的是节点的内部检查报告(使用**erlang:memory()** )会显示内存使用比操作系统的报告少很多。
</font> <p></p>

