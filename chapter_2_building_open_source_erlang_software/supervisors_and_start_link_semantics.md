# Supervisors and start_link Semantics
# 监控进程和start_link用法

In complex production systems, most faults and errors are transient, and retrying an operation is a good way to do things — Jim Gray’s paper 7 quotes Mean Times Between Failures (MTBF) of systems handling transient bugs being better by a factor of 4 when doing this.<br>
Still, supervisors aren’t just about restarting.
<p></p>
<font color="green">
&emsp;在一个复杂的产品系统中，大部分的错误(faults,errors)都是短暂的，重试无疑是一个好办法--Jim Gray's论文指出重启会4倍于系统的平均处理故障时间间隔(Mean Times Between Failures )。<br>
但是，supervisors并不是只会重启进程。
</font>
<p></p>
One very important part of Erlang supervisors and their supervision trees is that their start phases are synchronous. Each OTP process has the potential to prevent its siblings and cousins from booting.If the process dies, it’s retried again, and again, until it works, or fails too often.
<p></p>
<font color="green">
&emsp;Erlang supervisors和他们监控的supervisor树有一个非常重要的特性：他们的开始阶段是同步的，每个OTP进程都有可能阻止他的兄弟姐妹启动。 如果一个进程死掉，他会不断地重启，直到他正常或太多次失败后。
</font>
<p></p>
That’s where people make a very common mistake. There isn’t a backoff or cooldown period before a supervisor restarts a crashed child. When a network-based application tries to set up a connection during its initialization phase and the remote service is down, the application fails to boot after too many fruitless restarts. Then the system may shut down.
<p></p>
<font color="green">
&emsp;这就是人们常犯错误的地方。当supervisor重启一个崩溃的子进程时并没有补偿(backoff)或冷却时间(cooldown)。所以当一个基于网络的application尝试在初始化时建立一个连接，但远程的服务已挂掉了，那个这个applicaiton就会启动失败，并不断地重启，然后整个系统就崩溃。
</font>
<p></p>

Many Erlang developers end up arguing in favor of a supervisor that has a cooldown period. I strongly oppose the sentiment for one simple reason: **it’s all about the guarantees.**
<p></p>
<font color="green">

&emsp;许多的Erlang开发者都在争论是不是要给supervisor加一个冷却时间。但我强烈反对这种做法，因为一个非常简单的理由： **it’s all about the guarantees.**
</font>
<p></p>

[7] http://mononcqc.tumblr.com/post/35165909365/why-do-computers-stop
<p></p>
[注7]：http://mononcqc.tumblr.com/post/35165909365/why-do-computers-stop

