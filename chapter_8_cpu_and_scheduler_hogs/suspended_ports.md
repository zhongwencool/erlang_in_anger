# Suspended Ports
An interesting part of system monitors that didn’t fit anywhere but may have to do with
scheduling is regarding ports. When a process sends too many message to a port and the
port’s internal queue gets full, the Erlang schedulers will forcibly de-schedule the sender
until space is freed. This may end up surprising a few users who didn’t expect that implicit back-pressure from the VM.<br>
<p></p> <font color="green">
&emsp;关于系统监控(system monitors)的一个有趣事实就是：它不适合于任何地方，但却可能会参与调度相关的端口。当一个进程发送过多的信息给一个端口把端口的内部消息队列撑满时，Erlang调度器会强制重新分配发送者直到有再有空间。这就可以解释发生在back-pressure VM机器时的一些奇怪现象。
</font> <p></p>

&emsp;This kind of event can be monitored by passing in the atom **busy_port** to the system monitor. Specifically for clustered nodes, the atom **busy_dist_port** can be used to find when a local process gets de-scheduled when contacting a process on a remote node whose inter-node communication was handled by a busy port.<br>
&emsp;If you find out you’re having problems with these, try replacing your sending functions where in critical paths with **erlang:port_command(Port, Data, [nosuspend])** for ports, and **erlang:send(Pid, Msg, [nosuspend])** for messages to distributed processes. They will then tell you when the message could not be sent and you would therefore have been descheduled.
<p></p> <font color="green">
&emsp;这种类型的事件可以通过system monitor的**busy_port**参数来监控。特别是对于集群节点，**busy_dist_port**可以用于找到本地节点会被取消调度的时刻，当与一个远程节点通信时，本地使用的是非常忙的通信端口。<br>
&emsp;如果你发现你有这些问题,试着把你发信息了函数在关键的位置为端口加上**erlang:port_command(Port, Data, [nosuspend])**，并使用**erlang:send(Pid, Msg, [nosuspend])**来发送跨节点的发送。然后设置后他们会告诉你：在你消息无法发送时，你还一直在发，导致被重新分配了。
</font> <p></p>

