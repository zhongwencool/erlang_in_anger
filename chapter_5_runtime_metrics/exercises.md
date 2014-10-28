# Exercises
## Review Questions
## 复习问题
[1]. What kind of values are reported for Erlang’s memory?<br>
[2]. What’s a valuable process-related metric for a global view?<br>
[3]. What’s a port, and how should it be monitored globally?<br>
[4]. Why can’t you trust top or htop for CPU usage with Erlang systems? What’s the
alternative?<br>
[5]. Name two types of signal-related information available for processes<br>
[6]. How can you find what code a specific process is running?<br>
[7]. What are the different kinds of memory information available for a specific process?<br>
[8]. How can you know if a process is doing a lot of work?<br>
[9]. Name a few of the values that are dangerous to fetch when inspecting processes in a
production system.<br>
[10]. What are some features provided to OTP processes through the sys module?<br>
[11]. What kind of values are available when inspecting inet ports?<br>
[12]. How can you find the type of a port (Files, TCP, UDP)?<br>
<p></p> <font color="green">
[1] Erlang内存有那几个指标要关注的？<br>
[2] 全局指标里面有那些是进程相关的指标？<br>
[3] 端口是什么，为什么它要被作为全局监控指标？<br>
[4] 为什么不能相信top或htop中Erlang系统CPU的使用情况？<br>
[5] 列出两种与signal-releated相关的进程可用信息<br>
[6] 你是怎么判断进程在运行哪一些代码的？<br>
[7] 对于特定的进程，内存可用的信息有那几种？各自是什么作用？<br>
[8] 如果一个进程工作负载超载了，你通过什么知道？<br>
[9] 列出生产系统中查看一些进程时，存在安全隐患的指标？<br>
[10] OTP 进程通过sys模块会指供那一些特性？<br>
[11] 使用inet ports时可以查看到什么指标？<br>
[12] 你怎么确定一个端口的类型(Files,TCP,UDP)?<br>
</font> <p></p>
## Open-ended Questions
## 开放式问题
[1]. Why do you want a long time window available on global metrics?<br>
[2]. Which would be more appropriate between recon:proc_count/2 and recon:proc_window/3
to find issues with:<br>
(a) Reductions<br>
(b) Memory<br>
(c) Message queue length<br>
[3]. How can you find information about who is the supervisor of a given process?<br>
[4]. When should you use recon:inet_count/2? recon:inet_window/3?<br>
[5]. What could explain the difference in memory reported by the operating system and
the memory functions in Erlang?<br>
[6]. Why is it that Erlang can sometimes look very busy even when it isn’t?<br>
[7]. How can you find what proportion of processes on a node are ready to run, but can’t
be scheduled right away?<br>
<p></p> <font color="green">
[1] 你想使用窗口长期全局监控什么指标？<br>
[2] recon:proc_count/2 and recon:proc_window/3 那一个更能适合于找到以下场景的问题：<br>
a) 归约(reductions)<br>
b) 内存<br>
c) 消息队列长度<br>
[3] 怎么找到一个进程的监控进程(supervisor)相关信息？ <br>
[4] recon:inet_count/2? recon:inet_window/3分别在什么情况下使用?<br>
[5] 怎么样解释使用操作系统监控的内存使用情况与Erlang系统自己内部监控的内存使用之间的差异?<br>
[6] 为什么Erlang进程有时会在不忙时也看起来负荷很重?<br>
[7] 你是怎么知道一个节点上准备运行但是还没有被分配资源的进程比例的？<br>
</font> <p></p>


## Hands-On
##亲手实践
Using the code at https://github.com/ferd/recon_demo:<br>
[1]. What’s the system memory?<br>
[2]. Is the node using a lot of CPU resources?<br>
[3]. Is any process mailbox overflowing?<br>
[4]. Which chatty process (council_member) takes the most memory?<br>
[5]. Which chatty process is eating the most CPU?<br>
[6]. Which chatty process is consuming the most bandwidth?<br>
[7]. Which chatty process sends the most messages over TCP? The least?<br>
[8]. Can you find out if a specific process tends to hold multiple connections or file descriptors open at the same time on a node?<br>
[9]. Can you find out which function is being called by the most processes at once on the
node right now?<br>
<p></p> <font color="green">
使用这里的code: https://github.com/ferd/recon_demo:<br>
[1] 什么是系统内存？<br>
[2] 这个节点使用了大量的CPU资源么？<br>
[3] 有没有进程的信箱里面有大量消息<br>
[4] 哪个进程占用了大部分的内存<br>
[5] 哪个进程使用了大部分CPU资源<br>
[6] 哪个进程消耗了大部分的带宽<br>
[7] 那个进程发送了最多的TCP消息，最少的又是哪一个<br>
[8] 你可以找出哪一个进程在一个节点上同时打开了多个连接或文件描述符么？<br>
[9] 你可以找出大部分进程同时运行的是那一个函数么？<br>
</font> <p></p>






















