# Dealing With Constant Overload
# 处理恒定的过载
Being under constant overload may require a new solution. Whereas both queues and buffers will be great for cases where overload happens from time to time (even if it’s a rather prolonged period of time), they both work more reliably when you expect the input rate to eventually drop, letting you catch up.
<p></p> <font color="green">
针对恒定的超负荷，需要一个新的解决方案。鉴于队列和缓冲可以很好的应付不时发生的过载(即使这是一个相当长的时间)，使用他们能让你在输入低于最终期望时，再次迎头赶上来。
</font> <p></p>
You’ll mostly get problems when trying to send so many messages they can’t make it all to one process without overloading it.
<p></p> <font color="green">
当你试图发非常多的消息时(超过一个进程所能承受量)时一定会遇到麻烦。
</font> <p></p>
Two approaches are generally good for this case:<br>
• Have many processes that act as buffers and load-balance through them (scale horizontally)<br>
• use ETS tables as locks and counters (reduce the input)<br>
<p></p> <font color="green">
以下有2个方法非常适用于这种场景：
•  用更多的进程为它们来做缓冲(buffers)和负载均衡(扩张规模)<br>
•  使用ETS表来作锁(locks)和计数器(counters)，(减缓输入)<br>
</font> <p></p>

ETS tables are generally able to handle a ton more requests per second than a process, but the operations they support are a lot more basic. A single read, or adding or removing from a counter atomically is as fancy as you should expect things to get for the general case.<br>
ETS tables will be required for both approaches. Generally speaking, the first approach could work well with the regular process registry:<br>
<p></p> <font color="green">
ETS 表通常能比一个进程在每秒内处理更多的请求，而且ETS支持更多的基本操作，读，增或从计数器中自动删除都会和你平常的预期一样的。<br>
ETS表可以满足这两种方法。通常来讲，第一种方法(用更多的进程来做缓冲和负载均衡)可以在常规注册进程(the regular process registry)下工作得很好。
</font> <p></p>
you take N processes to divide up the load, give them all a known name, and pick one of them to send the message to. Given you’re pretty much going to assume you’ll be overloaded, randomly picking a process with an even distribution tends to be reliable: no state communication is required, work will be shared in a roughly equal manner, and it’s rather insensitive to failure.
<p></p> <font color="green">
你可以用N个进程来分担负载，给他们每个都海注册一个名字，用来区别发消息。鉴于你几乎已确定会超载，随机挑选一个进程来分配任务还是很靠谱的：通信必须要没有状态(进程间没有共享资源)，这样任务都是以相同的方式共享，并且对失败反应很不敏感。
</font> <p></p>
In practice, though, we want to avoid atoms generated dynamically, so I tend to prefer to register workers in an ETS table with read_concurrency set to true.<br>
 It’s a bit more work, but it gives more flexibility when it comes to updating the number of workers later on.
 <p></p> <font color="green">
在实践中，我们都会尽量禁止动态生成原子，所以我更喜欢把工作进程(Pid)都存在ETS表中，并把read_concurrency设置为true.<br>
还要做一点工作，要支持更新工作进程(当进程重启时，Pid变化后要更新等等)。<br>
</font> <p></p>

An approach similar to this one is used in the lhttpc <sup>22</sup> library mentioned earlier, to split load balancers on a per-domain basis.
<p></p> <font color="green">
与上面这个类似的方法：就是前面提到的lhttpc<sup>22</sup>，将负载均衡放在每一个基本域上(per-domain basis).
</font> <p></p>
For the second approach, using counters and locks, the same basic structure still remains (pick one of many options, send it a message), but before actually sending a message, you must atomically update an ETS counter <sup>23</sup>. There is a known limit shared across all clients (either through their supervisor, or any other config or ETS value) and each request that can be made to a process needs to clear this limit first.
<p></p> <font color="green">
那么第二个方法就是：使用计数器和锁机制，仍然是原来的基本数据结构(众多选项中保留一个发信息的选项)，但是在真正发消息之前，你必须要自动更新一下ETS的计数(counter)<sup>23</sup>.有一个已知的限制参数可以让所有的客户端拿到(不论你是存在他们的监控进程中，还是配置文件或ETS表中的值)，每一个请求处理前必须要通过这个限制。
</font> <p></p>
This approach has been used in dispcount <sup>24</sup> to avoid message queues, and to guarantee low-latency responses to any message that won’t be handled so that you do not need to wait to know your request was denied. It is then up to the user of the library whether to give up as soon as possible, or to keep retrying with different workers.
<p></p> <font color="green">
这个方法被dispcount<sup>24</sup>所使用：用来避免消息队列过长，同时也保证了可以很快地回应任何不能处理的消息，这样你就不需要等待(低延迟)知道你的请求被拒绝了，这取决于用户使用的库是不是会尽快放弃或在其它不再的工作进程中不断地再尝试。
</font> <p></p>

[22] The lhttpc_lb module in this library implements it.<br>
[23] By using ets:update_counter/3.<br>
[24] https://github.com/ferd/dispcount <br>
<p></p> <font color="green">
[注22] ：可以查看lhttpc_l模块实现了。<br>
[注23] :通过使用ets:update_counter/3。<br>
[注24] ：https://github.com/ferd/dispcount <br>
</font> <p></p>


