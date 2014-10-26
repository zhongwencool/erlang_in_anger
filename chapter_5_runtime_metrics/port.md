# Port
In a manner similar to processes, Ports should be considered. Ports are a datatype that
encompasses all kinds of connections and sockets opened to the outside world: TCP sockets,
UDP sockets, SCTP sockets, file descriptors, and so on.<br>
There is a general function (again, similar to processes) to count them: length(erlang:ports()) .
However, this function merges in all types of ports into a single entity. Instead, one can
use recon to get them sorted by type:<br>
<p></p> <font color="green">
端口数(Ports)也是跟进程数相类似的指标，端口是一个数据类型,他包含了连接和向外界开放的套接字：TCP 套接字,UDP套接字，SCTP套接字，文件描述符等。<br>
有一个通用的函数(和进程数的processes()类似)来计算它们：length(erlang:ports()).但这个函数把所有的端口类型都集中在了一起，你可以使用recon的port_type来根据类型排序他们。
</font> <p></p>
---------------------------------------------------------<br>
`1> recon:port_types().`<br>
`[{"tcp_inet",21480},`<br>
`{"efile",2},`<br>
`{"udp_inet",2},`<br>
`{"0/1",1},`<br>
`{"2/2",1},`<br>
`{"inet_gethost 4 ",1}]`<br>
--------------------------------------------------------<br>
This list contains the types and the count for each type of port. The type name is a
string and is defined by the Erlang VM itself.<br>
All the *_inet ports are usually sockets, where the prefix is the protocol used (TCP,
UDP, SCTP). The efile type is for files, while "0/1" and "2/2" are file descriptors for
standard I/O channels (stdin and stdout) and standard error channels (stderr), respectively.
Most other types will be given names of the driver they’re talking to, and will be
examples of port programs <sup>14</sup> or port drivers <sup>15</sup>.<br>
Again, tracking these can be useful to assess load or usage of a system, detect leaks,
and so on.

<p></p><font color="green">
上面这个列表包含了每种类型的端口和数量。端口类型名是一个Erlang VM自己定义的字符串。<br>
所有以XXX_inet的端口通常都是套接字，前缀就是所使用的协议(TCP,UDP,SCTP)。那个efile类型是针对文件的，"0/1" 和 "2/2" 就是对标准I/O(stdin和stdout),和错误处理stderr的文件描述符。<br>
大多数其它的类型在与驱动(dirver)交互时被给定一个名字，会成为prot programs<sup>14</sup>或prot drivers<sup>15</sup>的参照。<br>
和进程数一样，全程跟踪端口数会对分析负载或侦察进程泄漏有极大的帮助。
</font> <p></p>

[14] http://www.erlang.org/doc/tutorial/c_port.html<br>
[15] http://www.erlang.org/doc/tutorial/c_portdriver.html

<p></p> <font color="green">
[14] http://www.erlang.org/doc/tutorial/c_port.html<br>
[15] http://www.erlang.org/doc/tutorial/c_portdriver.html
</font> <p></p>

