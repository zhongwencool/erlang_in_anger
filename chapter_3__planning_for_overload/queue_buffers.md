# Queue Buffers
# 队列缓冲
Queue buffers are a good alternative when you want more control over the messages you get rid of than with random drops, particularly when you expect overload to be coming in bursts rather than a constant stream in need of thinning.
<p></p> <font color="green">
&emsp;当你不想随机丢弃消息而试图控制这更多的消息，那么可以选择队列缓冲。特别是你的消息流是不稳定的激增类型(不是源源不断的那种).
</font> <p></p>
Even though the regular mailbox for a process has the form of a queue, you’ll generally want to pull all the messages out of it as soon as possible. A queue buffer will need two processes to be safe:<br>
• The regular process you’d work with (likely a gen_server);<br>
• A new process that will do nothing but buffer the messages. Messages from the outside should go to this process.<br>

<p></p> <font color="green">
&emsp;即使通常的进程信箱都有队列，你平时只要把所有的消息都尽可能地放进去就行了。一个队列缓冲则需要2个进程都是安全的：<br>
&emsp;•  常规的工作进程(就像gen_server)。<br>
&emsp;•  一个只做缓冲消息工作的新进程，外界的消息应当先路由到这个进程中。
</font> <p></p>

To make things work, the buffer process only has to remove all the messages it can from its mail box and put them in a queue data structure <sup>16</sup> it manages on its own.
<p></p> <font color="green">
&emsp;为了让其工作正常，缓冲进程只需要删除所有来自自己信箱的消息，并把他们入到一个由它来管理的队列数据结构<sup>16</sup>中。
</font> <p></p>
Whenever the server is ready to do more work, it can ask the buffer process to send it a given number of messages that it can work on. The buffer process picks them from its queue, forwards them to the server, and goes back to accumulating data.
<p></p> <font color="green">
&emsp;只要工作进程已准备好可以接受更多的工作时，它就可以请求缓冲进程把定量的消息发给自己来处理。缓冲进程从队列中拿出这些消息，把它们交给工作进程，并回去堆积更多的消息。
</font> <p></p>
Whenever the queue grows beyond a certain size 17 and you receive a new message, you can then pop the oldest one and push the new one in there, dropping the oldest elements as you go. <sup>18</sup>
<p></p> <font color="green">
&emsp;当队列增长超过一定规模时<sup>17</sup>，这时你又收到了一个新消息，你可以弹出最老的消息，并把这个新来的消息放在那，甚至可以把最老的消息给丢弃掉<sup>18</sup>，这完全由你来设计。
</font> <p></p>
This should keep the entire number of messages received to a rather stable size and provide a good amount of resistance to overload, somewhat similar to the functional version of a ring buffer.<br>
The PO Box <sup>19</sup> library implements such a queue buffer.
<p></p> <font color="green">
&emsp;这就能要保证能处理稳定的消息流，并提供一个强抗过载能力，有点类似于循环缓冲(ring buffer)的功能版。

PO Box<sup>19</sup>就提供了这样一个缓冲队列。
[16] The queue module in Erlang provides a purely functional queue data structure that can work fine for such a buffer.<br>
[17] To calculate the length of a queue, it is preferable to use a counter that gets incremented and decremented on each message sent or received, rather than iterating over the queue every time. It takes slightly more memory, but will tend to distribute the load of counting more evenly, helping predictability and avoiding more sudden build-ups in the buffer’s mailbox. <br>
[18] You can alternatively make a queue that pops the newest message and queues up the oldest ones if you feel previous data is more important to keep.<br>
[19] Available at: https://github.com/ferd/pobox, the library has been used in production for a long time in large scale products at Heroku and is considered mature
<p></p> <font color="green">

[注16]：这个队列数据结构可由Erlang内部提供的。<br>
[注17]：计算消息的个数最后在收发消息时用一个计数器自己计数，不要每次遍历队列，这会使用更多的内存，并增加了不必要的负载。<br>
[注18]：如果你觉得那老的消息更重要，你还可以又建一个队列来处理新来的消息。<br>
[注19]： https://github.com/ferd/pobox 这个库很久之前大规模用于Heroku，是很成熟的库。<br>
</font> <p></p>

