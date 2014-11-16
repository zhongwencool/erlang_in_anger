# Tracing with Recon
Recon, by default, will match all processes. This will often be good enough for a lot
of debugging cases. The interesting part you’ll want to play with most of the time is
specification of trace patterns. Recon support a few basic ways to declare them.
<br>&emsp;The most basic form is {Mod, Fun, Arity}, where Mod is a literal module, Fun is a function name, and Arity is the number of arguments of the function to trace. Any of
these may also be replaced by wildcards (’_’ ). Recon will forbid forms that match too
widely on everything (such as {’_’,’_’,’_’}), as they could be plain dangerous to run in
production.
<br>&emsp;A fancier form will be to replace the arity by a function to match on lists of arguments. The function is limited to those usable by match specifications similar to what is available in ETS <sup>10</sup>. Finally, multiple patterns can be put into a list to broaden the matching scope.
<br>&emsp;It will also be possible to rate limit based on two manners: a static count, or a number of matches per time interval. Rather than going more in details, here’s list of examples and how to trace for them.<br>
<p></p> <font color="green">
&emsp;Recon默认会匹配(match)所有进程。这对于大量的debug场景都非常好。大多数情况下，你要处理的部分是特定的trace paterns。Recon支持一些基本的定义它们的方法。<br>
&emsp;最常用的形式就是 {Mod, Fun, Arity}，Mod就是模块名，Fun就是一个函数名，Arity就是需要trace的函数的参数个数。他们中的任意一个都可以使用通配符(’_’)来替换。Recon禁止匹配形式太过宽泛（比如
{’_’ , ’_’ , ’_’} ）,因为他们在生产环境下运行非常危险。<br>
</font> <p></p>
<p></p> <font color="green">
&emsp;一种更漂亮的形式就是通过一个函数匹配的参数列表来替换arity。函数被用match spcifications限制住，与ETS <sup>10</sup>表的match specifications 类似。最终，可以放入多个模式扩大匹配范围列表。<br>
&emsp;它还有可能基于两种方法限制速率：静态统计（static count） 和每个时间间隔的匹配数（a number of matches per time interval）。与其说更多细节，不如给出一系列的例子来说明如何trace他们。
</font> <p></p>

-----------------------------------------------------------------<br>
`%% All calls from the queue module, with 10 calls printed at most:`<br>
`recon_trace:calls({queue, ’_’, ’_’}, 10)%% All calls to lists:seq(A,B), with 100 calls`<br> `printed at most:`<br>
`recon_trace:calls({lists, seq, 2}, 100)`<br>
`%% All calls to lists:seq(A,B), with 100 calls per second at most:`<br>
`recon_trace:calls({lists, seq, 2}, {100, 1000})`<br>
`%% All calls to lists:seq(A,B,2) (all sequences increasing by two) with 100 calls`<br>
`%% at most:`<br>
`recon_trace:calls({lists, seq, fun([_,_,2]) -> ok end}, 100)`<br>
`%% All calls to iolist_to_binary/1 made with a binary as an argument already`<br>
`%% (a kind of tracking for useless conversions):`<br>
`recon_trace:calls({erlang, iolist_to_binary,`<br>
`fun([X]) when is_binary(X) -> ok end},`<br>
`10)`<br>
`%% Calls to the queue module only in a given process Pid, at a rate of 50 per`<br>
`%% second at most:`<br>
`recon_trace:calls({queue, ’_’, ’_’}, {50,1000}, [{pid, Pid}])`<br>
`%% Print the traces with the function arity instead of literal arguments:`<br>
`recon_trace:calls(TSpec, Max, [{args, arity}])`<br>
`%% Matching the filter/2 functions of both dict and lists modules, across new`<br>
`%% processes only:`<br>
`recon_trace:calls([{dict,filter,2},{lists,filter,2}], 10, [{pid, new]})`<br>
`%% Tracing the handle_call/3 functions of a given module for all new processes,`<br>
`%% and those of an existing one registered with gproc:`<br>
`recon_trace:calls({Mod,handle_call,3}, {1,100}, [{pid, [{via, gproc, Name}, new]}`<br>
`%% Show the result of a given function call, the important bit being the`<br>
`%% return_trace() call or the {return_trace} match spec value.`<br>
`recon_trace:calls({Mod,Fun,fun(_) -> return_trace() end}, Max, Opts)`<br>
`recon_trace:calls({Mod,Fun,[{’_’, [], [{return_trace}]}]}, Max, Opts)`<br>
-----------------------------------------------------------------<br>
<br>&emsp;Each call made will override the previous one, and all calls can be cancelled with **recon_trace: clear/0**.
<br>&emsp;There’s a few more combination possible, with more options:
<br>&emsp;**{pid, PidSpec}** Which processes to trace. Valid options is any of all, new, existing, or a process descriptor ({A,B,C}, "<A.B.C>", an atom representing a name, {global, Name},**{via, Registrar, Name}**, or a pid). It’s also possible to specify more than one by putting them in a list.
<br>&emsp;**{timestamp, format ter | trace}**
<br>&emsp;By default, the formatter process adds timestamps to messages received. If accurate timestamps are required, it’s possible to force the usage of timestamps within trace messages by adding the option {timestamp, trace}.
<br>&emsp;**{args, arity | args}**
<br>&emsp;Whether to print the arity in function calls or their (by default) literal representation.
<br>&emsp;**{scope, global | local}**
<br>&emsp;By default, only ’global’ (fully qualified function calls) are traced, not calls made internally. To force tracing of local calls, pass in {scope, local}. This is useful whenever you want to track the changes of code in a process that isn’t called with
**Module:Fun(Args)** , but just** Fun(Args)** .
<br>&emsp;With these options, the multiple ways to pattern match on specific calls for specific functions and whatnot, a lot of development and production issues can more quickly be diagnosed. If the idea ever comes to say "hm, maybe I should add more logging there to see what could cause that funny behaviour", tracing can usually be a very fast shortcut to get the data you need without deploying any code or altering its readability.
<p></p> <font color="green">
&emsp;每次调用都会覆盖前面的调用，所有的调用会可以用**recon_trace: clear/0**清除掉。
</font> <p></p>

<p></p> <font color="green">
&emsp;接下来说说更多选项可能组合成的结果：<br>
&emsp;**{pid, PidSpec}**trace特定的进程。有效的选项是 **all**，** new**， **existing**， 或一个进程描述符（**{A,B,C}**, "< A.B.C >"）, 一个代表进程的名字**atom** , **{global, Name}**,**{via, Registrar, Name}**, 或一个  **pid**)，可以把他们放在一个列表里面同时trace多个。<br>
&emsp;默认情况下,格式化处理程序将时间戳添加到收到的消息中。如果需要精确的时间戳,可以强制使用时间戳的trace消息选**项{timestamp, trace}**。<br>
&emsp;**{args, arity | args}**
<br>&emsp;是否打印函数调用中的参数数量(默认)文字表示。
<br>&emsp;**{scope, global | local}**
<br>&emsp;默认情况下,只有‘global‘(完全限定的函数调用fully qualified function calls)会被trace,不会调用内部的。如果强制trace本地调用，使用选项{scope, local}，这在你想trace进程中使用** Fun(Args)**调用(不是**Module:Fun(Args)**)的代码变化时非常有用。<br>
&emsp;有了这些参数，对于特定的函数调用就可以采用多种模式匹配结合使用的方式。很多开发问题或生产过程中的问题都可以更快地被诊断出来。如果以前的想法是”嗯,也许我应该添加更多的日志,看看可能会导致有趣的行为”，那些使用trace通常可以非常快获取您想要的数据而且不需要改动任何代码或改变它的可读性。
</font> <p></p>
[10] http://www.erlang.org/doc/man/ets.html#fun2ms-1
<p></p> <font color="green">
[注10] http://www.erlang.org/doc/man/ets.html#fun2ms-1
</font> <p></p>

