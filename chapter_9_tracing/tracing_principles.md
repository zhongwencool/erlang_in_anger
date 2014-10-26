# Tracing Principles
The Erlang Trace BIFs allow to trace any Erlang code at all <sup>8</sup>. They work in two parts: *pid
specifications*, and *trace patterns*.
<br>&emsp;**Pid** specifications lets the user decide which processes to target. They can be specific
pids, all pids, **existing** pids, or **new****** pids (those not spawned at the time of the function
call).
<br>&emsp;The trace patterns represent functions. Functions can be specified in two parts: specifying the modules, functions, and arity, and then with Erlang match specifications <sup>9</sup> to add
constraints to arguments.
<br>&emsp;What defines whether a specific function call gets traced or not is the intersection of
both, as seen in Figure 9.1.
If either the pid specification excludes a process or a trace pattern excludes a given call,
no trace will be received.
<br>&emsp;Tools like **dbg** (and trace BIFs) force you to work with this Venn diagram in mind.
You specify sets of matching pids and sets of trace patterns independently, and whatever
happens to be at the intersection of both sets gets to be displayed.
<br>&emsp;Tools like **redbug** and **recon_trace**, on the other hand, abstract this away.
![](http://erlang-in-anger.qiniudn.com/chapter9_1.png)
<br>&emsp;Figure 9.1: What gets traced is the result of the intersection between the matching pids
and the matching trace patterns
[8] In cases where processes contain sensitive information, data can be forced to be kept private by calling
process_flag(sensitive, true)<br>
[9] http://www.erlang.org/doc/apps/erts/match_spec.html<br>
