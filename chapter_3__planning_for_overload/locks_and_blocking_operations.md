# Locks and Blocking Operations
# 锁和阻塞操作
Locking and blocking operations will often be problematic when they’re taking unexpectedly long to execute in a process that’s constantly receiving new tasks.<br>
One of the most common examples I’ve seen is a process blocking while accepting a connection or waiting for messages with TCP sockets.
<p></p> <font color="green">
&emsp;锁和阻塞操作经常会执行出乎意料长的时间，随之新任务又源源不断地袭来，自然就会堆积成了问题。<br>
&emsp;一个我见过最常见的例子：一个进程阻塞着为了接受连接(accepting a connection)或等待TCP sockets消息。
</font> <p></p>
During blocking operations of this kind, messages are free to pile up in the message queue.<br>
One particularly bad example was in a pool manager for HTTP connections that I had written in a fork of the lhttpc library. It worked fine in most test cases we had, and we even had a connection timeout set to 10 milliseconds to be sure it never took too long <sup>3</sup>.
<p></p> <font color="green">
&emsp;在这种阻塞操作期间，大量其它消息都堆积在进程的消息队列中。<br>
&emsp;另一个是非常糟糕的例子：我写的一个lhttpc 库(a fork of the lhttpc)用于HTTP连接的进程池管理。 它在绝大多数test cases里都工作正常，我们甚至把一个连接的timeout设置为10ms，来确保它不会花太多时间。
</font> <p></p>

After a few weeks of perfect uptime, the HTTP client pool caused an outage when one of the remote servers went down.<br>
The reason behind this degradation was that when the remote server would go down, all of a sudden, all connecting operations would take at least 10 milliseconds, the time before which the connection attempt is given up on. With around 9,000 messages per second to the central process, each usually taking under 5 milliseconds, the impact became similar to
roughly 18,000 messages a second and things got out of hand.
<p></p> <font color="green">
&emsp;完美地运行了几个星期后，一个远程服务器崩溃了，引起了HTTP进程池(HTTP client pool)中断。<br>
&emsp;这次中断背后的原因是：当远程服务器挂掉后，突然所有的连接操作都要用至少10ms(放弃尝试连接的最小时间)的时间. 大约有9000条每秒的消息袭向中央进程，每个处理要花费5ms，这就相当于18000条每秒，然后服务器就失控啦。
</font> <p></p>
The solution we came up with was to leave the task of connecting to the caller process, and enforce the limits as if the manager had done it on its own. The blocking operations were now distributed to all users of the library, and even less work was required to be done by the manager, now free to accept more requests.
<p></p> <font color="green">
&emsp;我们随后的解决方案就是：让manager把连接工作交给工作调用进程，并让manager认为是自己已完成连接。这样阻塞操作就会被分发给所有的工作进程，manager需要做的活变少了，就可以处理更多的请求。
</font> <p></p>
When there is any point of your program that ends up being a central hub for receiving messages, lengthy tasks should be moved out of there if possible. Handling predictable overload <sup>4</sup> situations by adding more processes — which either handle the blocking operations or instead act as a buffer while the "main" process blocks — is often a good idea.
<p></p> <font color="green">
&emsp;尽可能不要把冗长的任务入在用于接受消息的中央枢纽中。一个常见的好方法：通过增加进程的方案来处理可预测的负载<sup>4</sup>。
&emsp;这些进程用于处理阻塞操作或作为主处理进程(中央枢纽)的一个缓冲。
</font> <p></p>
There will be increased complexity in managing more processes for activities that aren’t intrinsically concurrent, so make sure you need them before programming defensively.
<p></p> <font color="green">
&emsp;但你在编程前先考虑好是否需要它们，因为为了这本质上不是并发的活动而增加管理更多进程，要控制好复杂度。
</font> <p></p>
Another option is to transform the blocking task into an asynchronous one. If the type of work allows it, start the long-running job and keep a token that identifies it uniquely, along with the original requester you’re doing work for. When the resource is available, have it send a message back to the server with the aforementioned token. The server will eventually get the message, match the token to the requester, and answer back, without being blocked by other requests in the mean time. <sup>5</sup>
<p></p> <font color="green">
&emsp;另一种选择方案：把阻塞的任务变成异步操作。如果工作场景允许的话，开启一个长驻的进程，为每个请求都生成一个唯一标识(token)，并把请求按顺序分发给下面的进程(不用等返回)，当被分发的进程资源到位(成功处理)后，会把包含唯一token的成功消息作为异步应答返回给长驻进程，通知请求处理成功<sup>5</sup>。
</font> <p></p>
This option tends to be more obscure than using many processes and can quickly devolve into callback hell, but may use fewer resources.
<p></p> <font color="green">
&emsp;这种方案比使用更多的进程，并快速地转移到回调函数的方案更加晦涩，但是可能会占用更少资源。</font> <p></p>


[3] 10 milliseconds is very short, but was fine for collocated servers used for real-time bidding.<br>
[4] Something you know for a fact gets overloaded in production<br>
[5] The redo application is an example of a library doing this, in its redo_block module. The [underdocumented] module turns a pipelined connection into a blocking one, but does so while maintaining pipeline<br>
aspects to the caller — this allows the caller to know that only one call failed when a timeout occurs, not all of the in-transit ones, without having the server stop accepting requests.

<p></p> <font color="green">

[注3]：10ms是非常短，但这却只是用于配置用于实时竞争的服务器。<br>
[注4]：一些你了解的关于负载的事实。<br>
[注5]：redo application就是这样的示例，在redo_block模块中...The [underdocumented] module turns a pipelined connection into a blocking one, but does so while maintaining pipeline<br>
aspects to the caller — this allows the caller to know that only one call failed when a timeout occurs, not all of the in-transit ones, without having the server stop accepting requests.
</font> <p></p>

