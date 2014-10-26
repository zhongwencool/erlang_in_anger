# Memory Fragmentation
Memory fragmentation issues are intimately related to Erlang’s memory model, as described
in Section 7.3.2. It is by far one of the trickiest issues of running long-lived Erlang nodes
(often when individual node uptime reaches many months), and will show up relatively
rarely.
<br>&emsp;The general symptoms of memory fragmentation are large amounts of memory being allocated during peak load, and that memory not going away after the fact. The
damning factor will be that the node will internally report much lower usage (through
**erlang:memory()** ) than what is reported by the operating system.
