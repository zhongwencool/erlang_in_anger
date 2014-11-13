# CPU
Unfortunately for Erlang developers, CPU is very hard to profile. There are a few reasons
for this:<br>
&emsp;• The VM does a lot of work unrelated to processes when it comes to scheduling — high
scheduling work and high amounts of work done by the Erlang processes are hard to
characterize.<br>
&emsp;• The VM internally uses a model based on reductions, which represent an arbitrary
number of work actions. Every function call, including BIFs, will increment a process
reduction counter. After a given number of reductions, the process gets descheduled.<br>
&emsp;• To avoid going to sleep when work is low, the threads that control the Erlang schedulers will do busy looping. This ensures the lowest latency possible for sudden load
spikes. The VM flag +sbwt none|very_short|short|medium|long|very_long can
be used to change this value.<br>
&emsp;These factors combine to make it fairly hard to find a good absolute measure of how
busy your CPU is actually running Erlang code. It will be common for Erlang nodes in
production to do a moderate amount of work and use a lot of CPU, but to actually fit a
lot of work in the remaining place when the workload gets higher.<br>
&emsp;The most accurate representation for this data is the scheduler wall time. It’s an optional
metric that needs to be turned on by hand on a node, and polled at regular intervals. It
will reveal the time percentage a scheduler has been running processes and normal Erlang
code, NIFs, BIFs, garbage collection, and so on, versus the amount of time it has spent
idling or trying to schedule processes.<br>
<p></p> <font color="green">
对Erlang开发者来说，非常不幸的是，CPU指标非常难以验证。有以下几个原因：<br>
&emsp;+ VM在做大量调度工作或Erlang进程在做大量很难被测定(characterize)的工作时，VM会做很多与工作进程不相关的大量工作。<br>
&emsp;+ VM内部使用一个基于归约(reductions)的模型(可以表示任意数量的工作行为)。每个函数的调用，包括BIFs,这会增加一个进程的归约数(reduction counter)。进程被分配了一定数量的归约数后，会被切换到不执行的状态(descheduled)。
&emsp;+ 为了防止进程在负载低时休眠，控制Erlang调度的进程会频繁地在一个loop里面跑。这是为了确保在非常低负荷的情况下，实然忙起来时也能正常。可以用VM的+sbwt none|very_short|short|medium|long|very_long 选项来调整这个值。
</font> <p></p>
<p></p> <font color="green">
&emsp;这些因素使得制定一个完全掌握CPU真实运行Erlang代码时的负荷的方案变得非常困难。所以这只适用于在生产中常见的Erlang节点在做适量的工作和使用大量的CPU的情况，并不适用于高负载工作时。<br>
&emsp;可以提高CPU表现的就是调度器墙时间(scheduler wall time).这是一个可选指标，需要手动找开一个节点并做定期的轮询。它显示运行进程，正常的Erlang代码，NIFs,BIFs,垃圾回收等的时间与花在空转或试图调度进程时间的百分比。
</font> <p></p>

&emsp;The value here represents scheduler utilization rather than CPU utilization. The higher
the ratio, the higher the workload.<br>
&emsp;While the basic usage is explained in the Erlang/OTP reference manual <sup>13</sup>, the value
can be obtained by calling recon:<br>
<p></p> <font color="green">
这个值与其说代表CPU的使用情况，不如更准确地说是代表调度器使用情况 <sup>13</sup>。百分比越高，负荷越大。
</font> <p></p>
-----------------------------------------------------------------------<br>
`1> recon:scheduler_usage(1000).`<br>
`[{1,0.9919596133421669},`<br>
`{2,0.9369579039389054},`<br>
`{3,1.9294092120138725e-5},`<br>
`{4,1.2087551402238991e-5}]`<br>
-----------------------------------------------------------------------<br>
&emsp;The function recon:scheduler_usage(N) will poll for N milliseconds (here, 1 second)
and output the value of each scheduler. In this case, the VM has two very loaded schedulers (at 99.2% and 93.7% repectively), and two mostly unused ones at far below 1%. Yet, a tool like htop would report something closer to this for each core:
<p></p> <font color="green">
&emsp;recon:scheduler_usage(N)会做N毫秒间隔的轮询(这里是1s)，定时地把每个调度器的值都输出来。在这个例子中，VM有2个正在运行的调度器(一个99.2%,一个93.7%).还有2个大部分没有的（低于1%)。一个类似于htop的工具也可以提供类似每个核的指标：
</font> <p></p>
-----------------------------------------------------------------------<br>
`1 [||||||||||||||||||||||||| 70.4%]`<br>
`2 [||||||| 20.6%]`<br>
`3 [|||||||||||||||||||||||||||||100.0%]`<br>
`4 [|||||||||||||||| 40.2%]`<br>
-----------------------------------------------------------------------<br>
&emsp;The result being that there is a decent chunk of CPU usage that would be mostly free
for scheduling actual Erlang work (assuming the schedulers are busy waiting more than
trying to select tasks to run), but is being reported as busy by the OS.<br>
&emsp;Another interesting behaviour possible is that the scheduler usage may show a higher
rate (1.0) than what the OS will report. Schedulers waiting for os resources are considered utilized as they cannot handle more work. If the OS itself is holding up on non-CPU tasks
it is still possible for Erlang’s schedulers not to be able to do more work and report a full
ratio.<br>
&emsp;These behaviours may especially be important to consider when doing capacity planning,
and can be better indicators of headroom than looking at CPU usage or load.
<p></p> <font color="green">
&emsp;上面显示的结果，有相当多的CPU都用在了调度Erlang工作中(假设调度器都忙于等待多个需要运行的进程)，但确被OS报告成非常忙。<br>
&emsp;另一个非常有趣的行为可以是调度器的使用率可能会比OS报告中的数值高。调度器等待OS资源会被认为他们已不能处理更多的负载了。如果OS自己在执行一个不需要CPU的任务，Erlang的调度器有可能不能做更多的工作，并显示一个100%。<br>
&emsp;在做负载评估时要特别去考虑这些行为，比去看CPU使用量和负载，更能做出一个更好的指标空间。
</font> <p></p>

[13] http://www.erlang.org/doc/man/erlang.html#statistics_scheduler_wall_time

<p></p> <font color="green">
[注13]: http://www.erlang.org/doc/man/erlang.html#statistics_scheduler_wall_time
</font> <p></p>

