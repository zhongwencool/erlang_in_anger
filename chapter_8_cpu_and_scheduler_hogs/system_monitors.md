# System Monitors
If nothing seems to stand out through either profiling or checking reduction counts, it’s
possible some of your work ends up being done by NIFs, garbage collections, and so on.<br>
These kinds of work may not always increment their reductions count correctly, so they
won’t show up with the previous methods, only through long run times.
To find about such cases, the best way around is to use **erlang:system_monitor/2**, and
look for **long_gc** and** long_schedule**. The former will show whenever garbage collection ends up doing a lot of work (it takes time!), and the latter will likely catch issues with busy processes, either through NIFs or some other means, that end up making them hard to de-schedule. <sup>8</sup>
<br>&emsp;We’ve seen how to set such a system monitor In Garbage Collection in 7.1.5, but here’s
a different pattern <sup>9</sup> I’ve used before to catch long-running items:<br>
<p></p> <font color="green">
&emsp;如果通过profiling或检查reduction counts没有看出什么问题，可能就是你的某些工作最终被NIFs,垃圾回收完成了。<br>
&emsp;这种工作并不总是能增加它们的reductions count的，所以用之前的方法并不能看出问题所在，只能通过长时间的运行观察。为了对付这样的场景，最好的方法就是使用**erlang:system_monitor/2**,来查看 **long_gc** 和** long_schedule**.前者会显示垃圾回收时最终做的大量工作(会消耗时间！),后者会找出繁忙进程中可能的问题，不论是通过NIFs还是其它途径，最终都很难重分配(re-schedule)他们的工作。
</font> <p></p>

----------------------------------------------------------------<br>
`1> F = fun(F) ->`<br>
`receive`<br>
`{monitor, Pid, long_schedule, Info} ->`<br>
`io:format("monitor=long_schedule pid=~p info=~p~n", [Pid, Info]);`<br>
`{monitor, Pid, long_gc, Info} ->`<br>
`io:format("monitor=long_gc pid=~p info=~p~n", [Pid, Info])`<br>
`end,`<br>
`F(F)`<br>
`end.`<br>
`2> Setup = fun(Delay) -> fun() ->`<br>
`register(temp_sys_monitor, self()),`<br>
`erlang:system_monitor(self(), [{long_schedule, Delay}, {long_gc, Delay}]),`<br>
`F(F)`<br>
`end end.`<br>
`3> spawn_link(Setup(1000)).`<br>
`<0.1293.0>`<br>
`monitor=long_schedule pid=<0.54.0> info=[{timeout,1102},`<br>
`{in,{some_module,some_function,3}},`<br>
`{out,{some_module,some_function,3}}]`<br>
----------------------------------------------------------------<br>
<br>&emsp;Be sure to set the long_schedule and long_gc values to large-ish values that might be reasonable to you. In this example, they’re set to 1000 milliseconds. You can either kill the monitor by calling exit(whereis(temp_sys_monitor), kill) (which will in turn kill the shell because it’s linked), or just disconnect from the node (which will kill the process because it’s linked to the shell.)
<br>&emsp;This kind of code and monitoring can be moved to its own module where it reports to a long-term logging storage, and can be used as a canary for performance degradation or overload detection.
<p></p> <font color="green">
&emsp;先确认好你设置long_schedule and long_gc足够大。在上面这个例子中，设置为1000ms。你可以通过调用**exit(whereis(temp_sys_monitor), kill)**杀死这个监控进程(这也会杀死shell，因为他们linked在一起的)，或只是断开与节点(这会杀死监控进程，因为他与shell linked在一起的)。<br>
&emsp;上面这部分代码可以放到他自己的模块中，这样就可以把报告变成一个长期的日志存储，当作是性能下降或过载检测的分析依据。
</font> <p></p>
[8] Long garbage collections count towards scheduling time. It is very possible that a lot of your long schedules will be tied to garbage collections depending on your system.
[9] If you’re on 17.0 or newer versions, the shell functions can be made recursive far more simply by using their named form, but to have the widest compatibility possible with older versions of Erlang, I’ve let them
as is.
<p></p> <font color="green">
[注8]:垃圾回收是计入调度时间长的，这有可能导致：大量的调度任务会被垃圾回收(垃圾回收取决于系统)所束缚。<br>
[注9]:如果你使用的是17.0或更高的版本，shell函数可以递归，可以通过更简单的命名形式来写，但是为了兼容旧的版本，所以我还是按旧的方法来写。
</font> <p></p>

