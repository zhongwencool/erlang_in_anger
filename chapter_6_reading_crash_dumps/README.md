# Chapter 6 Reading Crash Dumps
# 读懂 Crash Dumps
Whenever an Erlang node crashes, it will generate a crash dump <sup>1</sup>.<br>
&emsp;The format is mostly documented in Erlang’s official documentation <sup>2</sup>, and anyone willing to dig deeper inside of it will likely be able to figure out what data means by looking
at that documentation. There will be specific data that is hard to understand without also
understanding the part of the VM they refer to, but that might be too complex for this
document.<br>
<p></p> <font color="green">
不管Erlang节点什么时间崩溃，都会生成一个crash dump<sup>1</sup>文件。<br>
&emsp;文件的格式说明大部分都在Erlang的官方文档<sup>2</sup>中，任何想深入了解里面的数据含意的，都可以通过查看这份文档。还有一些特定的数据，除非你理解VM相关的信息，才能看得懂，否则就非常难以理解，但对于这份文档可能太复杂了。
</font> <p></p>

&emsp;The crash dump is going to be named **erl_crash.dump** and be located wherever the Erlang process was running by default. This behaviour (and the file name) can be overridden
by specifying the **ERL_CRASH_DUMP** environment variable <sup>3</sup>.
<p></p> <font color="green">
&emsp;那个crash输出文件叫作：**erl_crash.dump**，默认在Erlang进程运行的地方生成。这个默认行为(和文件的名字)都可以通过改变**ERL_CRASH_DUMP**环境变量<sup>2</sup>做到。
</font> <p></p>

[1] If it isn’t killed by the OS for violating ulimits while dumping or didn’t segfault.<br>
[2] http://www.erlang.org/doc/apps/erts/crash_dump.html<br>
[3] Heroku’s Routing and Telemetry teams use the heroku_crashdumps app to set the path and name of the crash dumps. It can be added to a project to name the dumps by boot time and put them in a pre-set location

<p></p> <font color="green">
[1] 如果它不是被OS在dumping时暴力终结或段错误(didn't segfault).<br>
[2] http://www.erlang.org/doc/apps/erts/crash_dump.html<br>
[3] Heroku和Telemetry团队就是使用heroku_crashdumps 来设置这个路径和名称的环境变量。这个可以在项目启动时把他们预先设置好。
</font> <p></p>


