# Exercises
## Review Questions
[1]. What are the two main approaches to pin issues about CPU usages?<br>
[2]. Name some of the profiling tools available. What approaches are preferable for production use? Why?<br>
[3]. Why can long scheduling monitors be useful to find CPU or scheduler over-consumption?
<p></p> <font color="green">
[1].定位CPU使用相关的问题有哪 两种方法？<br>
[2].列出一些可用的profiling 工具。生产可能中，那一种方式更加适合？为什么？<br>
[3].为什么long scheduling monitors可以有效地找出CPU或调度器过度消费问题？<br>
</font> <p></p>

## Open-ended Questions
[1]. If you find that a process doing very little work with reductions ends up being scheduled for long periods of time, what can you guess about it or the code it runs?<br>
[2]. Can you set up a system monitor and then trigger it with regular Erlang code? Can
you use it to find out for how long processes seem to be scheduled on average? You
may need to manually start random processes from the shell that are more aggressive
in their work than those provided by the existing system.

<p></p> <font color="green">
[1].如果你发现一个进程运行了很长时间后结束是只有一点归约(reductions)时，你能猜出是什么？或者它运行的代码吗?<br>
[2].你可以使用常规的代码设置系统的监控和触发么？你能使用它来找出进程被调度的平均时长?你可能需要手动从shell中随机启动进程，这样会比已有系统提供的进程更加有效果<br>
</font> <p></p>

