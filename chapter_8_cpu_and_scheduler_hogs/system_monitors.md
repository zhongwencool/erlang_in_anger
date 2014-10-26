# System Monitors
If nothing seems to stand out through either profiling or checking reduction counts, it’s
possible some of your work ends up being done by NIFs, garbage collections, and so on.<br>
These kinds of work may not always increment their reductions count correctly, so they
won’t show up with the previous methods, only through long run times.
To find about such cases, the best way around is to use **erlang:system_monitor/2**, and
look for **long_gc** and** long_schedule**. The former will show whenever garbage collection
ends up doing a lot of work (it takes time!), and the latter will likely catch issues with
busy processes, either through NIFs or some other means, that end up making them hard
to de-schedule. <sup>8</sup>
<br>&emsp;We’ve seen how to set such a system monitor In Garbage Collection in 7.1.5, but here’s
a different pattern <sup>9</sup> I’ve used before to catch long-running items:<br>
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
<br>&emsp;Be sure to set the long_schedule and long_gc values to large-ish values that might be
reasonable to you. In this example, they’re set to 1000 milliseconds. You can either kill
the monitor by calling exit(whereis(temp_sys_monitor), kill) (which will in turn kill
the shell because it’s linked), or just disconnect from the node (which will kill the process
because it’s linked to the shell.)
<br>&emsp;This kind of code and monitoring can be moved to its own module where it reports to
a long-term logging storage, and can be used as a canary for performance degradation or
overload detection.

[8] Long garbage collections count towards scheduling time. It is very possible that a lot of your long
schedules will be tied to garbage collections depending on your system.
[9] If you’re on 17.0 or newer versions, the shell functions can be made recursive far more simply by using
their named form, but to have the widest compatibility possible with older versions of Erlang, I’ve let them
as is.

