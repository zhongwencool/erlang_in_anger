# Process
</font> <p></p>
By all means, processes are an important part of a running Erlang system. And because
they’re so central to everything that goes on, there’s a lot to want to know about them.
Fortunately, the VM makes a lot of information available, some of which is safe to use,
and some of which is unsafe to use in production (because they can return data sets large
enough that the amount of memory copied to the shell process and used to print it can kill the node).<br>
All the values can be obtained by calling process_info(Pid, Key) or
process_info(Pid, [Keys]) 17. Here are the commonly used keys <sup>18</sup>:<br>
 **Meta**<br>
dictionary returns all the entries in the process dictionary <sup>19</sup>. Generally safe to use, because people shouldn’t be storing gigabytes of arbitrary data in there.<br>
**group_leader** the group leader of a process defines where IO (files, output of io:format/1-3) goes. <sup>20</sup><br>
**registered_name** if the process has a name (as registered with erlang:register/2),
it is given here.<br>
**status** the nature of the process as seen by the scheduler. The possible values are:<br>
    &emsp;1. **exiting** the process is done, but not fully cleared yet;<br>
    &emsp;2. **waiting** the process is waiting in a receive ... end;<br>
    &emsp;3. **running** self-descriptive;<br>
    &emsp;4. **runnable** ready to run, but not scheduled yet because another process is running;<br>
    &emsp;5. **garbage_collecting** self-descriptive;<br>
    &emsp;6. **suspended** whenever it is suspended by a BIF, or as a back-pressure mechanism
because a socket or port buffer is full. The process only becomes runnable
again once the port is no longer busy.<br>
**Signals**<br>
&emsp;**links** will show a list of all the links a process has towards other processes and also
ports (sockets, file descriptors). Generally safe to call, but to be used with care
on large supervisors that may return thousands and thousands of entries.<br>
&emsp;**monitored_by** gives a list of processes that are monitoring the current process (through
the use of erlang:monitor/2).<br>
&emsp;**monitors** kind of the opposite of monitored_by; it gives a list of all the processes
being monitored by the one polled here.<br>
trap_exit has the value true if the process is trapping exits, false otherwise.
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
**Work**<br>
**reductions** The Erlang VM does scheduling based on reductions, an arbitrary unit
of work that allows rather portable implementations of scheduling (time-based
scheduling is usually hard to make work efficiently on as many OSes as Erlang
runs on). The higher the reductions, the more work, in terms of CPU and
function calls, a process is doing.<br>
Fortunately, for all the common ones that are also safe, recon contains the recon:info/1
function to help:<br>
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
For the sake of convenience, recon:info/1 will accept any pid-like first argument and
handle it: literal pids, strings ("<0.12.0>"), registered atoms, global names (**{global, Atom}**),
names registered with a third-party registry (e.g. with **gproc: {via, gproc, Name})**, or
tuples (**{0,12,0}**). The process just needs to be local to the node you’re debugging.<br>
If only a category of information is wanted, the category can be used directly:<br>
-------------------------------------------------------------------<br>
`2> recon:info(self(), work).`<br>
`{work,[{reductions,11035}]}`<br>
-------------------------------------------------------------------<br>
or can be used in exactly the same way as process_info/2:<br>
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
Any of the attributes mentioned earlier can work, and for nodes with long-lived processes
that can cause problems, it’s a fairly useful function.<br>
There is however a problem when most processes are short-lived, usually too short to
inspect through other tools, or when a moving window is what we need (for example, what
processes are busy accumulating memory or running code right now).<br>
For this use case, Recon has the **recon:proc_window(Attribute, Num, Milliseconds)**
function.<br>
It is important to see this function as a snapshot over a sliding window. A program’s
timeline during sampling might look like this:<br>
```
--w---- [Sample1] ---x-------------y----- [Sample2] ---z--->
```<br>

The function will take two samples at an interval defined by Milliseconds.<br>
Some processes will live between w and die at x, some between y and z, and some
between x and y. These samples will not be too significant as they’re incomplete.<br>
If the majority of your processes run between a time interval x to y (in absolute terms),
you should make sure that your sampling time is smaller than this so that for many processes, their lifetime spans the equivalent of **w** and **z**. Not doing this can skew the results:<br>
long-lived processes that have 10 times the time to accumulate data (say reductions) will
look like huge consumers when they’re not one. <sup>22</sup>
The function, once running gives results like follows:<br>
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
With these two functions, it becomes possible to hone in on a specific process that is
causing issues or misbehaving.<br>
[21] See Section 7.2<br>
[22] Warning: this function depends on data gathered at two snapshots, and then building a dictionary with
entries to differentiate them. This can take a heavy toll on memory when you have many tens of thousands
of processes, and a little bit of time.
<p></p> <font color="green">
</font> <p></p>
