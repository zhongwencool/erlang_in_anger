# Processes
</font> <p></p>
By all means, processes are an important part of a running Erlang system. And because
they’re so central to everything that goes on, there’s a lot to want to know about them.
Fortunately, the VM makes a lot of information available, some of which is safe to use,
and some of which is unsafe to use in production (because they can return data sets large
enough that the amount of memory copied to the shell process and used to print it can kill the node).<br>
All the values can be obtained by calling process_info(Pid, Key) or
process_info(Pid, [Keys]) <sup>17</sup>. Here are the commonly used keys <sup>18</sup>:<br>
<p></p> <font color="green">
&emsp;无论如何，进程信息(processes)都是运行时Erlang系统一个非常重要的指标。因为他们是一切的核心，关于他们，还有很多需要知道。幸运的是，VM虚拟机有很多有用的信息，一些可以安全使用，一些则是在生产环境时使用不安全的(因为他们可能会返回的数据非常大，千万大量的内存复制到shell进程中，打印这些数据会使用节点被崩溃掉)。<br>
&emsp;所有的值都可以使用process_info(Pid,Key)或process_info(pid,[<eys])<sup>17</sup>来查看。下面给出一些常用的keys<sup>18</sup>：<br>
</font> <p></p>

 **Meta**<br>
&emsp;1. **dictionary** returns all the entries in the process dictionary <sup>19</sup>. Generally safe to use, because people shouldn’t be storing gigabytes of arbitrary data in there.<br>
&emsp;2. **group_leader** the group leader of a process defines where IO (files, output of io:format/1-3) goes. <sup>20</sup><br>
&emsp;3. **registered_name** if the process has a name (as registered with erlang:register/2),
it is given here.<br>
&emsp;4. **status** the nature of the process as seen by the scheduler. The possible values are:<br>
    &emsp;a). **exiting** the process is done, but not fully cleared yet;<br>
    &emsp;b). **waiting** the process is waiting in a receive ... end;<br>
    &emsp;c). **running** self-descriptive;<br>
    &emsp;d). **runnable** ready to run, but not scheduled yet because another process is running;<br>
    &emsp;e). **garbage_collecting** self-descriptive;<br>
    &emsp;f). **suspended** whenever it is suspended by a BIF, or as a back-pressure mechanism
because a socket or port buffer is full. The process only becomes runnable
again once the port is no longer busy.<br>
<p></p> <font color="green">
&emsp;**Meta**<br>
&emsp;1.**dictionary** 返回进程中所有的进程字典值<sup>19</sup>。通常使用都安全，因为开发都不会把千兆级的数据入到进程字典里面。<br>
&emsp;2.**grpu_leader** 进程所属于的组(group),定义在IO输入输出的(files,使用io:format/1-3输入的地方)。<br>
&emsp;3.**status** 被调度器使用的进程属性值，可能有人值如下：<br>
&emsp;a) **exiting** 进程工作已完成，但是还没有全部被清理掉;<br>
&emsp;b) **waiting** 进程工作处于receive....end;<br>
&emsp;c) **running** 自我描述(self-descriptive);<br>
&emsp;d) **runnable** 进程已准备可以运行，但是还没有被分配，因为另一个进程在正在运行中(running);<br>
&emsp;e) **garbage_collection**自我描述(self-descriptive);<br>
&emsp;f) **suspended** 不论它是被BIF挂起，还是socket或port buffer都满负荷时被挂起的。这个进程只能在port有空时才会转为runnable状态。

</font> <p></p>

**Signals**<br>
&emsp;**links** will show a list of all the links a process has towards other processes and also
ports (sockets, file descriptors). Generally safe to call, but to be used with care
on large supervisors that may return thousands and thousands of entries.<br>
&emsp;**monitored_by** gives a list of processes that are monitoring the current process (through
the use of erlang:monitor/2).<br>
&emsp;**monitors** kind of the opposite of monitored_by; it gives a list of all the processes
being monitored by the one polled here.<br>
trap_exit has the value true if the process is trapping exits, false otherwise.

<p></p> <font color="green">
**Signals**<br>
&emsp;**link** 会显示写指定进程所links的所有其它进程列表，包括ports(sockets,file descriptors)。一般都是可以安全调用的，但在一个监控着成千上万个进程的大监控树进程来说，使用时要非常小心。<br>
&emsp;**monitored-by** 会返回监控本进程的所有进程列表(使用erlang:monitor/2做的监控)。<br>
&emsp;**monitors** 与monitored_by相反，返回指定进程监控的所有进程列表。<br>
</font> <p></p>
**Location**<br>
&emsp;**current_function** displays the current running function, as a tuple of the form
{Mod, Fun, Arity}. <br>
&emsp;**current_location** displays the current location within a module, as a tuple of the
form {Mod, Fun, Arity, [{File, FileName}, {line, Num}]}.<br>
&emsp;**current_stacktrace** more verbose form of the preceding option; displays the current
stacktrace as a list of ’current locations’.<br>
&emsp;**initial_call** shows the function that the process was running when spawned, of the
form {Mod, Fun, Arity}. This may help identify what the process was spawned
as, rather than what it’s running right now.<br>
<p></p> <font color="green">
**Signals**<br>
&emsp;**current_function** 显示当前运行的函数：返回值为{Mod, Fun, Arity}的元组。<br>
&emsp;**current_location** 显示当前运行的函数模块位置。返回值为：{Mod, Fun, Arity, [{File, FileName}, {line, Num}]}。<br>
&emsp;**current_stacktrace** 前缀选项里面一个非常详细选项;显示'current_locations'列表的当前堆栈(stacktrace)。<br>
*emsp;**initial_call** 显示进程运行spawned时初始化运行的函数：{Mod, Fun, Arity}.这可以用于帮助定位进程初始化时运行的函数，而不是进程当前运行的函数。<br>
</font> <p></p>

**Memory Used**<br>
&emsp;**binary Displays** the all the references to refc binaries <sup>21</sup> along with their size. Can be unsafe to use if a process has a lot of them allocated.<br>
&emsp;**garbage_collection** contains information regarding garbage collection in the process. The content is documented as ’subject to change’ and should be treated as
such. The information tends to contains entries such as the number of garbage
collections the process has went through, options for full-sweep garbage collections, and heap sizes.<br>
&emsp;**heap_size** A typical Erlang process contains an ’old’ heap and a ’new’ heap, and
goes through generational garbage collection. This entry shows the process’
heap size for the newest generation, and it usually includes the stack size. The
value returned is in words.<br>
&emsp;**memory** Returns, in bytes, the size of the process, including the call stack, the heaps,
and internal structures used by the VM that are part of a process.<br>
&emsp;**message_queue_len** Tells you how many messages are waiting in the mailbox of a
process.<br>
&emsp;**messages** Returns all of the messages in a process’ mailbox. This attribute is extremely dangerous to request in production because mailboxes can hold millions
of messages if you’re debugging a process that managed to get locked up. Always
call for the **message_queue_len** first to make sure it’s safe to use.<br>
&emsp;**total_heap_size** Similar to heap_size, but also contains all other fragments of the
heap, including the old one. The value returned is in words.<br>
<p></p> <font color="green">
**Memory Used**
<br>&emsp;**binary Displays** 所有的refc 类型的二进制的引用<sup>21</sup>和他的占用空间大小。如果进程有大量的refc类型的二进程，调用它就会非常不安全。<br>
&emsp;**garbage_collection** 进程关于垃圾回收的相关信息。文档中指出这个参数会有所变化('subject to change'),这些信息将来会包括进程所使用垃圾回收的次数，还有堆的大小。返回值为words类型。<br>
&emsp;**heap_size**一个典型的Erlang进程会包括‘old' heap和’new' heap和经过的垃圾回收器。这里只返回最新的堆大小和堆的大小，返回值为word类型。<br>
&emsp;**memory** 返回值为bytes类型，进程占用的内存大小，包含所有调用堆栈。<br>
&emsp;**message_queue_len** 进程中信箱消息个数总数。<br>
&emsp;**messages** 返回进程信箱中所有的信息。这个参数在生产环境中使用起来非常危险，因为在你一步步调试进程时，会把进程锁住，信箱可能会存着成千上万的消息。使用之前，请先使用**message_queue_len** 来确定下消息数量是不是很多。<br>
&emsp;**total_heap_size**与**heap_size**相类似，但会包括所有的堆信息，包括那个'old'heap,返回值为word类型。<br>

</font> <p></p>

**Work**<br>
**reductions** The Erlang VM does scheduling based on reductions, an arbitrary unit
of work that allows rather portable implementations of scheduling (time-based
scheduling is usually hard to make work efficiently on as many OSes as Erlang
runs on). The higher the reductions, the more work, in terms of CPU and
function calls, a process is doing.<br>
Fortunately, for all the common ones that are also safe, recon contains the recon:info/1
function to help:<br>
<p></p> <font color="green">
**Work**<br>
&emsp;**reductions** Erlang VM调度器是基于归约(reductions)，可以方便地调度任意单位的工作(基于时间的调度器通常都很难把工作做得跟Erlang一样有效率)。归约越高，进程消夏在CUP和函数调用的工作量越大。<br>
&emsp;幸运的是，这些常用的参数都是可以被安全调用的。你可以使用recon中的recon:info/1来查看：<br>
</font> <p></p>

-------------------------------------------------------------------<br>
`1> recon:info("<0.12.0>").` <br>
`[{meta,[{registered_name,rex},` <br>
`{dictionary,[{’$ancestors’,[kernel_sup,<0.10.0>]},` <br>
`{’$initial_call’,{rpc,init,1}}]},` <br>
`{group_leader,<0.9.0>},` <br>
`{status,waiting}]},` <br>
`{signals,[{links,[<0.11.0>]},` <br>
`{monitors,[]},` <br>
`{monitored_by,[]},` <br>
`{trap_exit,true}]},` <br>
`{location,[{initial_call,{proc_lib,init_p,5}},` <br>
`{current_stacktrace,[{gen_server,loop,6,` <br>
`[{file,"gen_server.erl"},{line,358}]},` <br>
`{proc_lib,init_p_do_apply,3,` <br>
`[{file,"proc_lib.erl"},{line,239}]}]}]},` <br>
`{memory_used,[{memory,2808},` <br>
`{message_queue_len,0},` <br>
`{heap_size,233},` <br>
`{total_heap_size,233},` <br>
`{garbage_collection,[{min_bin_vheap_size,46422},` <br>
`{min_heap_size,233},` <br>
`{fullsweep_after,65535},` <br>
`{minor_gcs,0}]}]},` <br>
`{work,[{reductions,35}]}]` <br>
-------------------------------------------------------------------<br>
&emsp;For the sake of convenience, recon:info/1 will accept any pid-like first argument and
handle it: literal pids, strings ("<0.12.0>"), registered atoms, global names (**{global, Atom}**),
names registered with a third-party registry (e.g. with **gproc: {via, gproc, Name}**), or
tuples (**{0,12,0}**). The process just needs to be local to the node you’re debugging.<br>
If only a category of information is wanted, the category can be used directly:<br>
<p></p> <font color="green">
&emsp;为了方便起见，recon:info/1可以接受任意像pid类型的参数：文字的pids,或字符串("<0.12.0>")，注册的原子，全局的消息名(**{global, Atom}**)，使用第三方的注册流程注册过的进程名(比如：**gproc: {via, gproc, Name}**)，或元组(**{0,12,0}**)。这些进程只需要是你调试的本地节点的进程就ok。如果你只需要某项信息，你可以使用下面的选项：
</font> <p></p>

-------------------------------------------------------------------<br>
`2> recon:info(self(), work).`<br>
`{work,[{reductions,11035}]}`<br>
-------------------------------------------------------------------<br>
or can be used in exactly the same way as process_info/2:<br>
<p></p> <font color="green">
或使用与process_info/2一样的选项：
</font> <p></p>

-------------------------------------------------------------------<br>
`3> recon:info(self(), [memory, status]).`<br>
`[{memory,10600},{status,running}]`<br>
-------------------------------------------------------------------<br>
This latter form can be used to fetch unsafe information.<br>
With all this data, it’s possible to find out all we need to debug a system. The challenge then is often to figure out, between this per-process data, and the global one, which
process(es) should be targeted.<br>
When looking for high memory usage, for example it’s interesting to be able to list all
of a node’s processes and find the top N consumers. Using the attributes above and the
recon:proc_count(Attribute, N) function, we can get these results:<br>
<p></p> <font color="green">
&emsp;后一种形式或能用于获取一些不安全的信息。<br>
&emsp;有了这些数据，我们就有可能找出调试系统中我们需要的所有信息。接下来的挑战往往是要找出这是得到个进程的数据，还是应该针对全局的某一个进程信息。<br>
&emsp;当发现是内存使用非常高时，比如，你可以列出列出节点所有进程并找出使用内存最高的前N个进程。使用recon:proce_count(Attribute,N)函数，我们就可以得到结果：<br>
</font> <p></p>

-------------------------------------------------------------------<br>
`4> recon:proc_count(memory, 3).`<br>
`[{<0.26.0>,831448,`<br>
`[{current_function,{group,server_loop,3}},`<br>
`{initial_call,{group,server,3}}]},`<br>
`{<0.25.0>,372440,`<br>
`[user,`<br>
`{current_function,{group,server_loop,3}},`<br>
`{initial_call,{group,server,3}}]},`<br>
`{<0.20.0>,372312,`<br>
`[code_server,`<br>
`{current_function,{code_server,loop,1}},`<br>
`{initial_call,{erlang,apply,2}}]}]`<br>
-------------------------------------------------------------------<br>
&emsp;Any of the attributes mentioned earlier can work, and for nodes with long-lived processes
that can cause problems, it’s a fairly useful function.<br>
&emsp;There is however a problem when most processes are short-lived, usually too short to
inspect through other tools, or when a moving window is what we need (for example, what
processes are busy accumulating memory or running code right now).<br>
For this use case, Recon has the **recon:proc_window(Attribute, Num, Milliseconds)**
function.<br>
It is important to see this function as a snapshot over a sliding window. A program’s
timeline during sampling might look like this:<br>
<p></p> <font color="green">
&emsp;前面提到的所有的选项都可以使用，对于节点中那些出问题的常驻进程，这是一个非常有用的函数。<br>
&emsp;但如果问题出在那些短暂生存的进程(short-lived)，通常会因为进程存活时间太短而不能被工具侦察到，或我们需要的是一个动态的窗口(比如：进程忙于计算内存或正好在运行代码)。<br>
&emsp;可以在这个动态的窗口中看到到这个函数的快照(snapshot)非常重要。在抽样进程的时间轴(timeline)可能看起来是这样子的：<br>
</font> <p></p>

```
--w---- [Sample1] ---x-------------y----- [Sample2] ---z--->
```<br>

&emsp;The function will take two samples at an interval defined by Milliseconds.<br>
&emsp;Some processes will live between w and die at x, some between y and z, and some
between x and y. These samples will not be too significant as they’re incomplete.<br>
If the majority of your processes run between a time interval x to y (in absolute terms),
you should make sure that your sampling time is smaller than this so that for many processes, their lifetime spans the equivalent of **w** and **z**. Not doing this can skew the results:
long-lived processes that have 10 times the time to accumulate data (say reductions) will
look like huge consumers when they’re not one. <sup>22</sup>
The function, once running gives results like follows:<br>
<p></p> <font color="green">
&emsp;该函数需要两个样本的时间间隔定义为毫秒。
&emsp;一些函数只存活在w与x之间，另一些函数存活在y和z之间，还有一些存活在x和y之间。<br>
&emsp;如果大部分进程都运行在x和y(按绝对值算),你就应该确保你的采样时间比这个小，来让他们一生可以跨越整个**w**~**z**生命周期。如果不这样做，就会使结果不准确：长驻进程可以有10次来累积数据(reductions)可能会被误认为是消耗大户<sup>22</sup>。
&emsp;这函数会返回出如下的结果：<br>

</font> <p></p>

-------------------------------------------------------------------<br>
`5> recon:proc_window(reductions, 3, 500).`<br>
`[{<0.46.0>,51728,`<br>
`[{current_function,{queue,in,2}},`<br>
`{initial_call,{erlang,apply,2}}]},`<br>
`{<0.49.0>,5728,`<br>
`[{current_function,{dict,new,0}},`<br>
`{initial_call,{erlang,apply,2}}]},`<br>
`{<0.43.0>,650,`<br>
`[{current_function,{timer,sleep,1}},`<br>
`{initial_call,{erlang,apply,2}}]}]`<br>
-------------------------------------------------------------------<br>
&emsp;With these two functions, it becomes possible to hone in on a specific process that is
causing issues or misbehaving.<br>
<p></p> <font color="green">
&emsp;有了这2个参数，就有可能定位到某个引起问题或行为不当的特定进程。<br>

</font> <p></p>

[17] In cases where processes contain sensitive information, data can be forced to be kept private by calling process_flag(sensitive, true) <br>
[18] For all options, look at http://www.erlang.org/doc/man/erlang.html#process_info-2<br>
[19] See http://www.erlang.org/course/advanced.html#dict and http://ferd.ca/on-the-use-of-the-processdictionary-in-erlang.html<br>
[20] See http://learnyousomeerlang.com/building-otp-applications#the-application-behaviour and http://erlang.org/doc/apps/stdlib/io_protocol.html for more details.<br>
[21] See Section 7.2<br>
[22] Warning: this function depends on data gathered at two snapshots, and then building a dictionary with
entries to differentiate them. This can take a heavy toll on memory when you have many tens of thousands
of processes, and a little bit of time.
<p></p> <font color="green">
[注17]：对于进程含有某些敏感信息的情况，数据会被强制保存为私有：使用process_flag(sesitive,true)<br>
[注18]：所有的选项查看这里：http://www.erlang.org/doc/man/erlang.html#process_info-2<br>
[注19]：查看：http://www.erlang.org/course/advanced.html#dict 和 http://ferd.ca/on-the-use-of-the-processdictionary-in-erlang.html<br>
[注20]：更多信息：http://learnyousomeerlang.com/building-otp-applications#the-application-behaviour 和 http://erlang.org/doc/apps/stdlib/io_protocol.html<br>
[注21]：参见章节7.2<br>
[注22]：警告：此函数依赖于数据聚焦在两个快照，然后建立一个字典来区分他们。如果你有成千上万的进程，就会产生非常大的内存消耗和一点点时间。<br>

</font> <p></p>
