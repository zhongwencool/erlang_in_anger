#  It’s About the Guarantees
Restarting a process is about bringing it back to a stable, known state. From there, things can be retried. When the initialization isn’t stable, supervision is worth very little.<br>

An initialized process should be stable no matter what happens. That way, when its siblings and cousins get started later on, they can be booted fully knowing that the rest of the system that came up before them is healthy.<br>
If you don’t provide that stable state, or if you were to start the entire system asynchronously, you would get very little benefit from this structure that a try ... catch in a loop wouldn’t provide.<br>
Supervised processes provide guarantees in their initialization phase, not a best effort.<br>
<p></p>
<font color="green">

&emsp;重启一个进程的目标是为了它回归已知的稳定状态。当进程的初始化都不稳定，supervision重启策略就不值一提了。<br>
&emsp;一个进程的初始化应当在任何情况下都非常稳定.这样的话，当他的兄弟姐妹都成功启动后，就可以完成肯定之前启动的系统都是正常健康的。<br>
&emsp;如果你不能确保稳定的状态，或你是异步启动整个系统，你就很难从一个在一个使用了try...catch循环结构中得到线索.<br>
&emsp;保证被监控的进程能在初始化中稳定启动非常好，但这并不是最好的策略。<br>
</font>
<p></p>
This means that when you’re writing a client for a database or service, you shouldn’t need a connection to be established as part of the initialization phase unless you’re ready to say it will always be available no matter what happens.<br>
You could force a connection during initialization if you know the database is on the same host and should be booted before your Erlang system, for example. Then a restart should work.<br>
In case of something incomprehensible and unexpected that breaks these guarantees, the node will end up crashing, which is desirable: a pre-condition to starting your system hasn’t been met.<br>
 It’s a system-wide assertion that failed.
<p></p>
<font color="green">
&emsp;这意味着，当你为一个数据库或服务(database or service)写客户端时，你不能在初始化中建立连接，除非你已准备处理所有会发生的情况了。<br>
&emsp;如果你确定数据库在同一个主机下，并会在Erlang系统启动时就已启动了，那们你可以在初始化中强制建立连接。<br>
&emsp;当某些不可思议(incomprehensible and unexpected)的情况破坏了这种稳定性，这个节点就会崩溃，但正是我们期望的。因为并没有满足启动你的系统的前提条件。<br>
&emsp;这是一个系统级的错误。
</font> <p></p>

If, on the other hand, your database is on a remote host, you should expect the connection to fail. It’s just a reality of distributed systems that things go down <sup>8</sup>.<br>
In this case, the only guarantee you can make in the client process is that your client will be able to handle requests, but not that it will communicate to the database. It could return **{error, not_connected}** on all
calls during a net split, for example.

<p></p> <font color="green">
&emsp;但另一方面，如果你的数据库在别一个远程主机上，你应该要处理好当连接失败的情况，因为在真实的分布式系统中经常会发生这种情况<sup>8</sup>。<br>
&emsp;在这种情况下，唯一能做的保证就是：在客户端进程中，你的客户端有能力处理请求，而不是与数据库通信。比如：它会网络不通时在返回**{error,not_connected}**。
</font> <p></p>

 The reconnection to the database can then be done using whatever cooldown or backoff strategy you believe is optimal, without impacting the stability of the system. It can be attempted in the initialization phase as an optimization, but the process should be able to reconnect later on if anything ever disconnects.<br>
 If you expect failure to happen on an external service, do not make its presence a guarantee of your system. We’re dealing with the real world here, and failure of external dependencies is always an option.
 <p></p> <font color="green">

&emsp;不管你用什么手段(冷却时间或你认为最佳的补偿策略)，都必须确保数据库的重连，以便不影响整个系统的稳定性。可以尝试在初始化中做一个优化，但进程应该能在断开连接的情况下能重连回来。<br>
&emsp;你不要期望一个有保证的系统中的失败只发生在一个外部的服务上(an external service)。我们现在处理的是真实的世界，失败的外部依赖(failure of external dependencies )是常见的情景。
</font> <p></p>
[8] Or latency shoots up enough that it is impossible to tell the difference from failure.
<p></p>
<p></p> <font color="green">
[注8]：或网络延迟得很利害，以至于和失败没什么两样。
</font> <p></p>

