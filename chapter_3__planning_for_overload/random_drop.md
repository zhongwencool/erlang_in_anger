# Random Drop
# 随机丢弃
Randomly dropping messages is the easiest way to do such a thing, and might also be the most robust implementation, due to its simplicity.<br>
The trick is to define some threshold value between 0.0 and 1.0 and to fetch a random number in that range:
<p></p> <font color="green">
&emsp;随机丢弃消息是最容易做的事了，但也是正是由于他的简单，导致可能也是最粗暴的实现方式。<br>
&emsp;这个方法的难点在于如何定义0.0~1.0之间的阀值，并随机取范围内的一个数据：
</font> <p></p>
-------------------------------------------------------------------—----<br>
`-module(drop).`<br>
`-export([random/1]).`<br>
`random(Rate) ->`<br>
`maybe_seed(),`<br>
`random:uniform() =< Rate.`<br>
`maybe_seed() ->`<br>
`case get(random_seed) of`<br>
`undefined -> random:seed(erlang:now());`<br>
`{X,X,X} -> random:seed(erlang:now());`<br>
`_ -> ok`<br>
`end.`<br>
-------------------------------------------------------------------—----<br>
<p></p>

If you aim to keep 95% of the messages you send, the authorization could be written by a call to case drop:random(0.95) of true -> send(); false -> drop() end, or a shorter drop:random(0.95) andalso send() if you don’t need to do anything specific when dropping a message.
<p></p> <font color="green">

&emsp;如果你想要保持收到95%消息,那么就可以调用<br>
-------------------------------------------------------------------—----<br>
`case drop:random(0.95) of`<br>
`     ture -> send();`<br>
`     false -> drop()`<br>
`end.`<br>
-------------------------------------------------------------------—----<br>
&emsp;如果你不需要在丢弃时做任何时，就可以更简洁一些：<br>
-------------------------------------------------------------------—----<br>
`drop:random(0.95) andalso send().`<br>
-------------------------------------------------------------------—----<br>
</font> <p></p>
The maybe_seed() function will check that a valid seed is present in the process dictionary and use it rather than a crappy one, but only if it has not been defined before, in order to avoid calling now() (a monotonic function that requires a global lock) too often.<br>
There is one ‘gotcha’ to this method, though: the random drop must ideally be done at the producer level rather than at the queue (the receiver) level.
<p></p> <font color="green">
&emsp;maybe_seed() 函数会在进程字典里面检查是否存在一个有效的种子，并使用它。但这只针对种子已被定义过的情况，用来避免每次都要调用一个now()(这个单调函数是有一个全局锁的)<br>
这个方法里面有一个'gotcha',试想：这个随机丢弃必须要生产消息时就完成，而不是在接收消息时才丢弃。
</font> <p></p>

The best way to avoid overloading a queue is to not send data its way in the first place. Because there are no bounded mailboxes in Erlang, dropping in the receiving process only guarantees that this process will be spinning wildly, trying to get rid of messages, and fighting the schedulers to do actual work.<br>

On the other hand, dropping at the producer level is guaranteed to distribute the work equally across all processes.
<p></p> <font color="green">
&emsp;防止队列过载的最好方法是：最初就不要给它发消息。因为Elrang的信箱并没有限制大小，如果是在接收消息时才丢弃就只能保证这个进程会疯狂地运转来试图驾驭这些消息，并且要做调度工作来丢弃消息。<br>
&emsp;另一方面，在生产消息时就丢弃能保证在所有进程都是均等工作的。
</font> <p></p>
This can give place to interesting optimizations where the working process or a given monitor process<sup>15</sup> uses values in an ETS table or application:set_env/3 to dynamically increase and decrease the threshold to be used with the random number.
<p></p> <font color="green">
&emsp;关于怎么控制工作进程或一个给定的监控进程中的那个丢弃数据比例值，可以把它存入在一个ETS表里面或使用applicaiton:set_env/3来动态增加或减少这个比例<sup>15</sup>。
</font> <p></p>
This allows control over how many messages are dropped based on overload, and the configuration data can be fetched by any process rather efficiently by using application:get_env/2.<br>

Similar techniques could also be used to implement different drop ratios for different message priorities, rather than trying to sort it all out at the consumer level.
<p></p> <font color="green">

&emsp;这个比例要设定为多少是基于负荷的，使用application:get_env/2来得到数据比把这个值存在配置文件中让所有的进程中能取到的方法高效得多。<br>
&emsp;类似的技术可以实现对不到的消息优先级设定不同的丢弃率，而不是统一标准解决一切。
</font> <p></p>

[15] Any process tasked with checking the load of specific processes using heuristics such as process_info(Pid, message_queue_len) could be a monitor

<p></p> <font color="green">
[注15]：任何进程都可以使用如process_info(Pid,message_queue_len)的函数来监控另一个进程，所以就叫监控进程。
</font> <p></p>


