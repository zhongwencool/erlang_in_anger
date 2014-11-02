# Profiling and Reduction Counts
To pin issues to specific pieces of Erlang code, as mentioned earlier, there are two main
approaches. One will be to do the old standard profiling routine, likely using one of the
following applications: <sup>2</sup>
<br>&emsp;• **eprof**, <sup>3</sup> the oldest Erlang profiler around. It will give general percentage values and will mostly report in terms of time taken.
<br>&emsp;• **fprof**, <sup>4</sup> a more powerful replacement of eprof. It will support full concurrency and generate in-depth reports. In fact, the reports are so deep that they are usually considered opaque and hard to read.
<br>&emsp;• **eflame**, <sup>5</sup> the newest kid on the block. It generates flame graphs to show deep call sequences and hot-spots in usage on a given piece of code. It allows to quickly find issues with a single look at the final result.
<br>&emsp;It will be left to the reader to thoroughly read each of these application’s documentation.
The other approach will be to run recon:proc_window/3 as introduced in Subsection 5.2.1:

<p></p> <font color="green">
&emsp;就像前面所说，为了定位到问题的具体Erlang代码，这也有两种方法。一个针对旧的标准分析程序的，可能使用了下面的application <sup>2</sup>:<br>
&emsp;• **eprof** <sup>3</sup>,最古老的Erlang分析工具。它可以得到通用的百分比数据和以terms形式返回的报告时间。<br>
<br>&emsp;• **fprof**<sup>4</sup>,一个强有力取代eprof的工具。它支持完整的并发和生成深度报告。事实上，报告嵌套非常深,通常被认为是不透明的,难以阅读。<br>
<br>&emsp;• **eflame** <sup>5</sup> ,最新的分析工具。它可以生成图表形式，用来显示指定代码的所有的调用序列(call sequences)和使用热点(hot-spots in usage)。它可以让你只简单的看下最终结果就能快速找到问题。<br>
&emsp;读者仔细阅读每一个application的文档哦。<br>
&emsp;另一个方法就是如章节5.2.1中所说的调用用recon:proc_window/3:<br>
</font> <p></p>

--------------------------------------------------<br>
`1> recon:proc_window(reductions, 3, 500).`<br>
`[{<0.46.0>,51728,`<br>
`[{current_function,{queue,in,2}},`<br>
`{initial_call,{erlang,apply,2}}]},`<br>
`{<0.49.0>,5728,`<br>
`[{current_function,{dict,new,0}},`<br>
`{initial_call,{erlang,apply,2}}]},`<br>
`{<0.43.0>,650,`<br>
`[{current_function,{timer,sleep,1}},`<br>
`{initial_call,{erlang,apply,2}}]}]`<br>
--------------------------------------------------<br>

<br>&emsp;The reduction count has a direct link to function calls in Erlang, and a high count is usually the synonym of a high amount of CPU usage.
<br>&emsp;What’s interesting with this function is to try it while a system is already rather busy, <sup>6</sup>
with a relatively short interval. Repeat it many times, and you should hopefully see a
pattern emerge where the same processes (or the same kind of processes) tend to always
come up on top.
<br>&emsp;Using the code locations <sup>7</sup> and current functions being run, you should be able to identify
what kind of code hogs all your schedulers.
<p></p> <font color="green">
&emsp;Erlang中的reduction count有一个直接的link通向调用的函数，一个高的计数通常等价于高数量的CPU被使用。<br>
&emsp;在系统已非常忙时<sup>6</sup>，如果这个函数要做一些有趣的尝试，在相对较短的时间间隔内重复多次，你就可以看到相同的进程(或相同类型的进程)会跑到最顶部来。<br>
&emsp;使用代码定位<sup>7</sup>和当前进行的函数，你就可以定位到你的所有分配者(all your schedulers)处于什么的代码问题(code hogs)。
</font> <p></p>


[2] All of these profilers work using Erlang tracing functionality with almost no restraint. They will havean impact on the run-time performance of the application, and shouldn’t be used in production.<br>
[3] http://www.erlang.org/doc/man/eprof.html<br>
[4] http://www.erlang.org/doc/man/fprof.html<br>
[5] https://github.com/proger/eflame<br>
[6] See Subsection 5.1.2<br>
[7] Call recon:info(PidTerm, location) or process_info(Pid, current_stacktrace)
to get this information.<br>

<p></p> <font color="green">
[注2]：所有这些Erlang分析器使用跟踪功能几乎没有限制。他们会对application运行时的性能有影响,不应该在生产系统中使用。<br>
[注3]： http://www.erlang.org/doc/man/eprof.html<br>
[注4]： http://www.erlang.org/doc/man/fprof.html<br>
[注5]： https://github.com/proger/eflame<br>
[注6]： 查看章节5.1.2<br>
[注7]：调用 recon:info(PidTerm, location) 或 process_info(Pid, current_stacktrace)来得到这些信息<br>
</font> <p></p>

