# Time-Sensitive Buffers
# 对时间敏感的缓冲区
If you need to react to old events before they are too old, then things become more complex, as you can’t know about it without looking deep in the stack each time, and dropping from the bottom of the stack in a constant manner gets to be inefficient.
<p></p> <font color="green">
&emsp;如果你需要在赶在事件变得过时之前处理掉，这就会变得更加复杂，如果是堆栈区，你只能每次都深入里面才能知道他的情况，从堆栈底部不断地得到元素是效率低下的。
</font> <p></p>
An interesting approach could be done with buckets, where multiple stacks are used, with each of them containing a given time slice. When requests get too old for the QoS constraints, drop an entire bucket, but not the entire buffer.
<p></p> <font color="green">
&emsp;下面介绍一个有意思实现方法：使用多堆栈，每个都包含一个给定的时间片(time slice).当请求的时间超过约束时间时(Qos)，把整个bucket都丢弃掉，注意不是整个缓冲区。
</font> <p></p>
It may sound counter-intuitive to make some requests a lot worse to benefit the majority — you’ll have great medians but poor 99 percentiles — but this happens in a state where you would drop messages anyway, and is preferable in cases where you do need low latency.
<p></p> <font color="green">
&emsp;这听起来有悖常理：让一些请求失败来造福其余的大多数----你会有一个非常好的medians便只能达到可怜的99%以下的处理指标，但这只是针对一个无论如何都要丢弃消息的状态，特别是你要保证低延迟的场合。
