# Ports
Similarly to processes, Erlang ports allow a lot of introspection. The info can be accessed
by calling **erlang:port_info(Port, Key)** , and more info is available through the inet
module. Most of it has been regrouped by the recon:port_info/1-2 functions, which
work using a somewhat similar interface to their process-related counterparts.<br>
<p></p> <font color="green">
&emsp;同进程的选项类似，Erlang的端口(ports)也允许非常多的内部数据查看。这些信息可以调用**erlang:prot_info(Port,Key)**来得到，其它更多信息可以通过inet模块得到。大部分的都被recon:prot_info/1-2函数所包含了，它们都是调用想类似的接口来实现的。
</font> <p></p>

**Meta**<br>
&emsp;**id** internal index of a port. Of no particular use except to differentiate ports.<br>
&emsp;**name** type of the port — with names such as "tcp_inet", "udp_inet", or "efile",
for example.<br>
&emsp;**os_pid** if the port is not an inet socket, but rather represents an external process or
program, this value contains the os pid related to the said external program.<br>
<p></p> <font color="green">
**Meta**<br>
&emsp;**id** : 端口的内部索引。除了用于区分端口外没有其它特殊用途。<br>
&emsp;**name**: 端口的类型--比如：可以是"tcp_inet","upd_inet"或"efile"。<br>
&emsp;**os_pid**: 如果端口不是一个inet socket,而是代表一个外部进程或程序，这个值就是来表示他是一个外部程序的os pid。<br>
</font> <p></p>

**Signals**<br>
&emsp;**connected** Each port has a controlling process in charge of it, and this process’ pid
is the connected one.<br>
&emsp;**links** ports can be linked with processes, much like other processes can be. The list
of linked processes is contained here. Unless the process has been owned by or
manually linked to a lot of processes, this should be safe to use.<br>
&emsp;**monitors** ports that represent external programs can have these programs end up
monitoring Erlang processes. These processes are listed here.<br>
<p></p> <font color="green">
**Signals**<br>
&emsp;**connected**: 每个端口都被一个控制进程所负责，这个就是控制进程的pid。<br>
&emsp;**links**: 端口可以link到进程，和其它进程一样的。返回它link的进程列表。在进程linked了很多进程时调用是不安全的。<br>
&emsp;**monitors**:端口可代表着外部程序，返回这个端口最终被那些Erlang进程监控。<br>
</font> <p></p>
**IO**<br>
&emsp;**input** the number of bytes read from the port.<br>
&emsp;**output** the number of bytes written to the port.<br>

<p></p> <font color="green">
**IO**<br>
&emsp;**input**: 从端口读取的数据大小(bytes)。<br>
&emsp;**output**: 写入端口的数据大小(bytes)。<br>
</font> <p></p>
**Memory Used**<br>
&emsp;**memory** this is the memory (in bytes) allocated by the runtime system for the port.
This number tends to be small-ish and excludes space allocated by the port itself.
&emsp;**queue_size** Port programs have a specific queue, called the driver queue <sup>24</sup>. This return the size of this queue, in bytes.<br>
<p></p> <font color="green">
**Memory Used**<br>
&emsp;**memory** : 运行时Erlang系统为端口分配的内存大小(bytes)。这个值往往会小于实际值，它忽略了端口自身的分配空间。<br>
&emsp;**queue_size**: 端口程序有一个特定的队列：叫做dirver queue<sup>24</sup>。返回这个队列的大小(bytes)。<br>
</font> <p></p>

**Type-Specific**<br>
&emsp;**Inet Ports** Returns inet-specific data, including statistics <sup>25</sup>, the local address and
port number for the socket (sockname), and the inet options used <sup>26</sup>.<br>
&emsp;**Others** currently no other form than inet ports are supported in recon, and an empty
list is returned.

<p></p> <font color="green">
**Type-Specific**<br>
&emsp;**Inet Ports**: 返回inet-specific数据。包括statistics<sup>25</sup>：本地地址,sockname的端口数，和inet的使用的选项<sup>26</sup>。<br>
&emsp;**Others**:目前还没有其它选项recon可以支持，只返回一个空列表。
</font> <p></p>
The list can be obtained as follows:<br>
<p></p> <font color="green">
可以获取以下类似的列表：<br>
</font> <p></p>

---------------------------------------------------------<br>
`1> recon:port_info("#Port<0.818>").`<br>
`[{meta,[{id,6544},{name,"tcp_inet"},{os_pid,undefined}]},`<br>
`{signals,[{connected,<0.56.0>},`<br>
`{links,[<0.56.0>]},`<br>
`{monitors,[]}]},`<br>
`{io,[{input,0},{output,0}]},`<br>
`{memory_used,[{memory,40},{queue_size,0}]},`<br>
`{type,[{statistics,[{recv_oct,0},`<br>
`{recv_cnt,0},`<br>
`{recv_max,0},`<br>
`{recv_avg,0},`<br>
`{recv_dvi,...},`<br>
`{...}|...]},`<br>
`{peername,{{50,19,218,110},80}},`<br>
`{sockname,{{97,107,140,172},39337}},`<br>
`{options,[{active,true},`<br>
`{broadcast,false},`<br>
`{buffer,1460},`<br>
`{delay_send,...},`<br>
`{...}|...]}]}]`<br>
---------------------------------------------------------<br>
&emsp;On top of this, functions to find out specific problematic ports exist the way they do
for processes. The gotcha is that so far, recon only supports them for inet ports and
with restricted attributes: the number of octets (bytes) sent, received, or both (**send_oct**, **recv_oct**, **oct**, respectively), or the number of packets sent, received, or both (**send_cnt**, **recv_cnt**, **cnt**, respectively).<br>
&emsp;So for the cumulative total, which can help find out who is slowly but surely eating up
all your bandwidth:<br>
<p></p> <font color="green">
&emsp;除此之外，函数可以找到具体存在问题的端口。目前为止，recon 只支持inet 端口的下面属性：octets发送接收的数据大小(bytes)(**send_oct**, **recv_oct**, **oct**),或packets发送接收的数据大小(**send_cnt**, **recv_cnt**, **cnt**)。<br>
&emsp; 这些数据的累积，可以帮助你找出那一个端口慢并耗尽了你的带宽。<br>
</font> <p></p>

--------------------------------------------------------<br>
`2> recon:inet_count(oct, 3).`<br>
`[{#Port<0.6821166>,15828716661,`<br>
`[{recv_oct,15828716661},{send_oct,0}]},`<br>
`{#Port<0.6757848>,15762095249,`<br>
`[{recv_oct,15762095249},{send_oct,0}]},`<br>
`{#Port<0.6718690>,15630954707,`<br>
`[{recv_oct,15630954707},{send_oct,0}]}]`<br>
--------------------------------------------------------<br>
&emsp;Which suggest some ports are doing only input and eating lots of bytes. You can then
use **recon:port_info("#Port<0.6821166>")** to dig in and find who owns that socket, and
what is going on with it.<br>
&emsp;Or in any other case, we can look at what is sending the most data within any time
window <sup>27</sup> with the** recon:inet_window(Attribute, Count, Milliseconds)** function:<br>
<p></p> <font color="green">
&emsp;可以发现一些端口只输入并吃掉了大量的bytes。你可以使用**recon:port_info("#Port<0.6821166>")**来挖出它所属于的socket，找出它为什么这样的原因。<br>
&emsp;或者其它情况，我们可以使用窗口<sup>27</sup>全程查看数据的流向：** recon:inet_window(Attribute, Count, Milliseconds)**
</font> <p></p>

--------------------------------------------------------<br>
`3> recon:inet_window(send_oct, 3, 5000).`<br>
`[{#Port<0.11976746>,2986216,[{send_oct,4421857688}]},`<br>
`{#Port<0.11704865>,1881957,[{send_oct,1476456967}]},`<br>
`{#Port<0.12518151>,1214051,[{send_oct,600070031}]}]`<br>
--------------------------------------------------------<br>
&emsp;For this one, the value in the middle of the tuple is what send_oct was worth (or any
chosen attribute for each call) during the specific time interval chosen (5 seconds here).<br>
&emsp;There is still some manual work involved into properly linking a misbehaving port to a
process (and then possibly to a specific user or customer), but all the tools are in place.<br>
<p></p> <font color="green">
&emsp;对于上面这个示例，元组中间的那个值就是在特定时间内(在这里是5s)使用send_oct(或任意使用这个属性的调用)发送的数据大小。<br>
&emsp;还有一些涉及到link到行为不当的函数的手动工作(可能是一个特定的用户),但所有的工作都在已列这里啦。
</font> <p></p>

[24] The driver queue is available to queue output from the emulator to the driver (data from the driver to the emulator is queued by the emulator in normal Erlang message queues). This can be useful if the driver has to wait for slow devices etc, and wants to yield back to the emulator.<br>
[25] http://www.erlang.org/doc/man/inet.html#getstat-1<br>
[26] http://www.erlang.org/doc/man/inet.html#setopts-2<br>
[27] See the explanations for the recon:proc_window/3 in the preceding subsection


<p></p> <font color="green">
[注24]：driver queue在从模拟器(emulator)输出到dirver的数据时有用，数据从dirver到模拟器的队列使用的是正常的Erlang消息队列。如果dirver处理数据快一些时，它就会屈服(yield back)于模拟器。<br>
[注25]：http://www.erlang.org/doc/man/inet.html#getstat-1<br>
[注26]：http://www.erlang.org/doc/man/inet.html#setopts-2<br>
[注27]：可以在前面的面容中查看到recon:proc_window/3解释<br>

</font> <p></p>

