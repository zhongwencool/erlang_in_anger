# error_logger Explodes
# 激增的error_logger
Ironically, the process in charge of error logging is one of the most fragile ones. In a default Erlang install, the error_logger <sup>2</sup> process will take its sweet time to log things to disk or over the network, and will do so much more slowly than errors can be generated.<br>
This is especially true of user-generated log messages (not for errors), and for crashes in large processes. For the former, this is because error_logger doesn’t really expect arbitrary levels of messages coming in continually. It’s for exceptional cases only and doesn’t expect lots of traffic. For the latter, it’s because the entire state of processes (including their mailboxes) gets copied over to be logged.<br>
It only takes a few messages to cause memory to bubble up a lot, and if that’s not enough to cause the node to run Out Of Memory (OOM), it may slow the logger enough that additional messages will.<br>
The best solution for this at the time of writing is to use lager as a substitute logging library.
<p></p> <font color="green">
&emsp;有点戏剧化的是，负责处理error 日志的进程是最脆弱的一员。对于一个默认安装的Erlang来说，error_logger<sup>2</sup>进程会把日志记录到硬盘或网络上，但记录日志操作的速度常常会赶不上错误产生的速度。<br>
&emsp;特别是在用户产生的日志消息(不是错误error)和大量进程崩溃的场合。对于前者（用户产生的日志）, error_logger从未考虑过大量消息同时记录的场景，因为这场景设定只在少量traffic的特殊情况下才会发生; 对于后者（大量进程崩溃），进程的全部信息(包括进程的信箱)都会被记录下来。<br>
&emsp;只要几个这种类型的消息就可以导致内存暴增，如果这还不足以导致内存耗尽(Out Of Memory,OOM),额外的信息也有可能也会减缓日志记录进程。<br>
&emsp;使用lager 替换日志库是目前为止最好的解决方案。
</font> <p></p>

While lager will not solve all your problems, it will truncate voluminous log messages, optionally drop OTP-generated error messages when they go over a certain threshold, and will automatically switch between asynchronous and synchronous modes for user-submitted messages in order to self-regulate.<br>
It won’t be able to deal with very specific cases, such as when user-submitted messages are very large in volume and all coming from one-off processes. This is, however, a much rarer occurrence than everything else, and one where the programmer tends to have more control.
<p></p> <font color="green">
&emsp;当lager解决不了你所有问题时，它会截断冗长的日志消息，并在超过某个阈值时就会选择性地忽略OTP自身产生的错误信息，并且为了自我调节，会根据用户提交的信息自动在同步和异步模式中切换。
<br>
&emsp;它不会处理那些非常特殊的情况，比如：用户产生的信息非常大并所有的信息都来自于一次性进程(one-off processes).因为这毕竟很少会发生，一旦发生，开发者往往会想出更多的控制方法。
</font> <p></p>
[2] Defined at http://www.erlang.org/doc/man/error_logger.html

<p></p> <font color="green">

[注2] :定义在http://www.erlang.org/doc/man/error_logger.html
</font> <p></p>
