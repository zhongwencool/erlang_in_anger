# Stack Buffers
# 堆栈缓冲区
Stack buffers are ideal when you want the amount of control offered by queue buffers, but you have an important requirement for low latency.<br>
To use a stack as a buffer, you’ll need two processes, just like you would with queue buffers, but a list 20 will be used instead of a queue data structure.
<p></p> <font color="green">
&emsp;堆栈缓冲区适用场景：当你给想坐拥队列缓冲区提供的大量控制权，但你又要比这更重要的低延迟要求。<br>
&emsp;使用堆栈来作缓冲，你也需要2个进程，就和队列缓冲一样的，但一个列表(List)。
</font> <p></p>
The reason the stack buffer is particularly good for low latency is related to issues similar to buffer bloat <sup>21</sup>. If you get behind on a few messages being buffered in a queue, all the messages in the queue get to be slowed down and acquire milliseconds of wait time.<br>
Eventually, they all get to be too old and the entire buffer needs to be discarded.
<p></p> <font color="green">
&emsp;堆栈缓冲特别好的一个重要原因是：不容易引起缓冲膨胀相关的问题<sup>21</sup>。如果你想从缓冲队列中得到几个后面的消息，所有的在队列中的消息处理都会变慢并且需要毫秒级的等待时间。<br>
&emsp;最终，他们会因为滞后太久，整个缓冲区都要被丢弃掉。
</font> <p></p>

On the other hand, a stack will make it so only a restricted number of elements are kept waiting while the newer ones keep making it to the server to be processed in a timely manner.<br>
Whenever you see the stack grow beyond a certain size or notice that an element in it is too old for your QoS requirements you can just drop the rest of the stack and keep going from there. PO Box also offers such a buffer implementation.
<p></p> <font color="green">
&emsp;而另一方面，堆栈缓冲区会对保持等待的消息数量有一个严格的限制，并可以让服务器及时处理新来的。<br>
&emsp;当你看到堆栈缓冲区已增长到一定大小或注意到里面的元素质量达不到你需求的服务标准(QoS)，你就可以把堆栈余下的部分都丢弃掉，然后再继续工作，POBox就实现了这样的缓冲buffer。
</font> <p></p>

A major downside of stack buffers is that messages are not necessarily going to be processed in the order they were submitted — they’re nicer for independent tasks, but will ruin your day if you expect a sequence of events to be respected.
<p></p> <font color="green">
&emsp;堆栈缓冲区主要的缺点就是消息并不是按他们提交顺序依次处理的，他们对独立的任务支持非常好，但你千万不要期望它按先来先处理的原则执行。
</font> <p></p>

[20] Erlang lists are stacks. For all we care, they provide push and pop operations that take O(1) complexity and are very fast.<br>
[21] http://queue.acm.org/detail.cfm?id=2071893

<p></p> <font color="green">
[注20]：Erlang List就是一个堆栈，我们都关心地是他提供压栈和弹出操作且只有O(1)的复杂度。<br>
[注21]：http://queue.acm.org/detail.cfm?id=2071893
</font> <p></p>
