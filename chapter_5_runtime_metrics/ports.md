# Ports
Similarly to processes, Erlang ports allow a lot of introspection. The info can be accessed
by calling erlang:port_info(Port, Key) , and more info is available through the inet
module. Most of it has been regrouped by the recon:port_info/1-2 functions, which
work using a somewhat similar interface to their process-related counterparts.<br>
**Meta**<br>
&emsp;**id** internal index of a port. Of no particular use except to differentiate ports.<br>
&emsp;**name** type of the port — with names such as "tcp_inet", "udp_inet", or "efile",
for example.<br>
&emsp;**os_pid** if the port is not an inet socket, but rather represents an external process or
program, this value contains the os pid related to the said external program.<br>
**Signals**<br>
&emsp;**connected** Each port has a controlling process in charge of it, and this process’ pid
is the connected one.<br>
&emsp;**links** ports can be linked with processes, much like other processes can be. The list
of linked processes is contained here. Unless the process has been owned by or
manually linked to a lot of processes, this should be safe to use.<br>
&emsp;**monitors** ports that represent external programs can have these programs end up
monitoring Erlang processes. These processes are listed here.<br>
**IO**
&emsp;**input** the number of bytes read from the port.
&emsp;**output** the number of bytes written to the port.
**Memory Used**<br>
&emsp;**memory** this is the memory (in bytes) allocated by the runtime system for the port.
This number tends to be small-ish and excludes space allocated by the port itself.
&emsp;**queue_size** Port programs have a specific queue, called the driver queue <sup>24</sup>. This
return the size of this queue, in bytes.
**Type-Specific**
&emsp;**Inet Ports** Returns inet-specific data, including statistics <sup>25</sup>, the local address and
port number for the socket (sockname), and the inet options used <sup>26</sup>.<br>
&emsp;**Others** currently no other form than inet ports are supported in recon, and an empty
list is returned.
The list can be obtained as follows:<br>

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
On top of this, functions to find out specific problematic ports exist the way they do
for processes. The gotcha is that so far, recon only supports them for inet ports and
with restricted attributes: the number of octets (bytes) sent, received, or both (**send_oct**, **recv_oct**, **oct**, respectively), or the number of packets sent, received, or both (**send_cnt**, **recv_cnt**, **cnt**, respectively).<br>
So for the cumulative total, which can help find out who is slowly but surely eating up
all your bandwidth:<br>
--------------------------------------------------------<br>
`2> recon:inet_count(oct, 3).`<br>
`[{#Port<0.6821166>,15828716661,`<br>
`[{recv_oct,15828716661},{send_oct,0}]},`<br>
`{#Port<0.6757848>,15762095249,`<br>
`[{recv_oct,15762095249},{send_oct,0}]},`<br>
`{#Port<0.6718690>,15630954707,`<br>
`[{recv_oct,15630954707},{send_oct,0}]}]`<br>
--------------------------------------------------------<br>
Which suggest some ports are doing only input and eating lots of bytes. You can then
use **recon:port_info("#Port<0.6821166>")** to dig in and find who owns that socket, and
what is going on with it.<br>
Or in any other case, we can look at what is sending the most data within any time
window <sup>27</sup> with the** recon:inet_window(Attribute, Count, Milliseconds)** function:<br>
--------------------------------------------------------<br>
`3> recon:inet_window(send_oct, 3, 5000).`<br>
`[{#Port<0.11976746>,2986216,[{send_oct,4421857688}]},`<br>
`{#Port<0.11704865>,1881957,[{send_oct,1476456967}]},`<br>
`{#Port<0.12518151>,1214051,[{send_oct,600070031}]}]`<br>
--------------------------------------------------------<br>
For this one, the value in the middle of the tuple is what send_oct was worth (or any
chosen attribute for each call) during the specific time interval chosen (5 seconds here).<br>
There is still some manual work involved into properly linking a misbehaving port to a
process (and then possibly to a specific user or customer), but all the tools are in place.<br>
<p></p>
[24] The driver queue is available to queue output from the emulator to the driver (data from the driver to
the emulator is queued by the emulator in normal Erlang message queues). This can be useful if the driver
has to wait for slow devices etc, and wants to yield back to the emulator.<br>
[25] http://www.erlang.org/doc/man/inet.html#getstat-1<br>
[26] http://www.erlang.org/doc/man/inet.html#setopts-2<br>
[27] See the explanations for the recon:proc_window/3 in the preceding subsection
