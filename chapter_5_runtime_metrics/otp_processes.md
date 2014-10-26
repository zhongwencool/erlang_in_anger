# OTP Processes
When processes in question are OTP processes (most of the processes in a production
system should definitely be OTP processes), you instantly win more tools to inspect them.
&emsp;In general the sys module <sup>23</sup> is what you want to look into. Read the documentation<br>
on it and you’ll discover why it’s so useful. It contains the following features for any OTP
process:<br>
<p></p> <font color="green">
&emsp;当发生问题的进程是OTP进程时(大部分产品级的系统都是OTP 进程),你就可以立即得到更多的工具来检查它们。<br>
&emsp;一般你可以在sys<sup>23</sup>模块找到你想要的。查阅它的文档，你会发现这什么它这么有用啦。它包括了针对OTP进程的以下特性：
</font> <p></p>

&emsp;• logging of all messages and state transitions, both to the shell or to a file, or even in
an internal buffer to be queried;<br>
&emsp;• statistics (reductions, message counts, time, and so on);<br>
&emsp;• fetching the status of a process (metadata including the state);<br>
&emsp;• fetching the state of a process (as in the #state{} record);<br>
&emsp;• replacing that state<br>
&emsp;• custom debugging functions to be used as callbacks<br>
&emsp;It also provides functionality to suspend or resume process execution.<br>
&emsp;I won’t go into a lot of details about these functions, but be aware that they exist.
<p></p> <font color="green">
&emsp;•记录了所有消息和状态转换，可以输出来shell上或指定文件中，甚至可以输出到一个内部 缓冲区(internal buffer)供查询。<br>
&emsp;•统计(reductions,消息数，时间等);<br>
&emsp;•得到进程的状态(包括数据状态);<br>
&emsp;•得到进程的状态(在#state{}中的数据);<br>
&emsp;•替换进程的状态;<br>
&emsp;•自定义的函数作为回调函数;<br>
&emsp;它还提供函数可以把进程挂起或使进程恢复至运行状态。<br>
&emsp;我不会在这里去讨论具体的函数，只是提醒下他们确实存在。

</font> <p></p>



[23] http://www.erlang.org/doc/man/sys.html
<p></p> <font color="green">
[23] http://www.erlang.org/doc/man/sys.html
</font> <p></p>
