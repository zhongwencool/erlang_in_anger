# Processes
Trying to get a global view of processes is helpful when trying to assess how much work
is being done in the VM in terms of tasks. A general good practice in Erlang is to use
processes for truly concurrent activities — on web servers, you will usually get one process
per request or connection, and on stateful systems, you may add one process per-user —
and therefore the number of processes on a node can be used as a metric for load.<br>
&emsp;Most tools mentioned in section 5.1 will track them in one way or another, but if the
process count needs to be done manually, calling the following expression is enough:<br>
<p></p> <font color="green">
&emsp;查看全局进程数对了解VM负荷非常有帮助。一个良好的Erlang实践就是为每一个并发的活动都派生出一个进程来处理--对于web服务器，每一个请求或连接就是一个进程，在保存状态的(statefull)系统中，你可能会为每一个用户都派生一个进程，因此可以把节点上的总进程数做为负载指标。<br>
&emsp;章节5.1中提到的大部分工具都能以不同的方式来追踪它们，但如果手动来计算进程数，你只需要使用下来这个函数就足够了：
</font> <p></p>
--------------------------------------------<br>
`1> length(processes()).`<br>
`56535`<br>
-------------------------------------------<br>
&emsp;Tracking this value over time can be extremely helpful to try and characterize load or
detect process leaks, along with other metrics you may have around.
<p></p> <font color="green">
&emsp;全程追踪这个指标会对衡量负载大小或侦察进程泄露，都非常有用，当然也可能会结合其它指标来一起诊断。
</font> <p></p>
