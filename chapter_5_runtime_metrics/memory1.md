# Memory
The memory reported by the Erlang VM in most tools will be a variant of what is reported by **erlang:memory()** :
<p></p> <font color="green">
Erlang VM大多数工具的内存报告都是通**过erlang:memory()**的变种实现的。
</font> <p></p>

-------------------------------------------------------------------------------------<br>
`1> erlang:memory().`<br>
`[{total,13772400},`<br>
`{processes,4390232},`<br>
`{processes_used,4390112},`<br>
`{system,9382168},`<br>
`{atom,194289},`<br>
`{atom_used,173419},`<br>
`{binary,979264},`<br>
`{code,4026603},`<br>
`{ets,305920}]`<br>
--------------------------------------------------------------------------------------<br>
<p></p>
&emsp;This requires some explaining.<br>
&emsp;First of all, all the values returned are in bytes, and they represent memory allocated
(memory actively used by the Erlang VM, not the memory set aside by the operating system
for the Erlang VM). It will sooner or later look much smaller than what the operating system reports.<br>
&emsp;The total field contains the sum of the memory used for processes and system (which
is incomplete, unless the VM is instrumented!). processes is the memory used by Erlang
processes, their stacks and heaps. system is the rest: memory used by ETS tables, atoms
in the VM, refc binaries<sup>11</sup>, and some of the hidden data I mentioned was missing.<br>
&emsp;If you want the total amount of memory owned by the virtual machine, as in the amount
that will trip system limits (ulimit), this value is more difficult to get from within the VM.<br>
&emsp;If you want the data without calling top or htop, you have to dig down into the VM’s memory allocators to find things out<sup>12</sup>.
<p></p> <font color="green">
&emsp;这里需要解释下：<br>
&emsp;首先，所有的返回值都是字节(bytes)为单位的，他们表示内存被分配(Erlang VM实际使用的内存，不是操作系统给Erlang VM分配的内存)。所以它迟早总会比操作系统报告的内存小得多（随意VM的版本更新，总会把Erlang VM使用的内存优化使用量更少一些）。<br>
&emsp;总字段包含了所有进程和系统(除instrumented模式外，其它并不完整!)的总内存占用大小。返回的processes项是指Erlang进程使用的堆栈总内存。system项就包含其余的：ETS表，VM中的原子，二进制的引用<sup>11</sup>(refc),和一些我没有提及到的隐藏数据。<br>
&emsp;如果你想得到VM占用的总内存，这个值在访问系统的限制下(ulimit)（由于不同的操作系统限制有差异）,很难从VM内部获得。<br>
&emsp;如果想不调用top或htop命令来得到数据，你就不得不自己深入VM内存管理分配来找到你想要的<sup>12</sup>。
</font> <p></p>
&emsp;Fortunately, recon has the function recon_alloc:memory/1 to figure it out, where the
argument is:<br>
&emsp;• **used** reports the memory that is actively used for allocated Erlang data;<br>
&emsp;• **allocated** reports the memory that is reserved by the VM. It includes the memory
used, but also the memory yet-to-be-used but still given by the OS. This is the amount
you want if you’re dealing with ulimit and OS-reported values.<br>
&emsp;• **unused** reports the amount of memory reserved by the VM that is not being allocated.
Equivalent to allocated-used.<br>
&emsp;• **usage** returns a percentage (0.0 .. 1.0) of used over allocated memory ratios.
There are additional options available, but you’ll likely only need them when investigating memory leaks in chapter 7
<p></p> <font color="green">
&emsp;不过，很幸运的是，recon有一个函数**：recon_alloc:memory/1**可以得到以下指标：<br>
&emsp;+  **used** 用于分配Erlang数据的内存使用报告;<br>
&emsp;+ **allocated** VM自己保留的内存分配报告。它包括了内存的使用，也包括还已由OS分配给VM但没有被使用的内存。如果你和ulimit和OS-reported值打交道，这些值非常有用。<br>
&emsp;+ **unused** VM保留但没有被分配的没使用内存报告。<br>
&emsp;+ **usage** 返回值可能是一个百分比(0.0..1.0)的内存分配比率值。<br>
&emsp;还有一些附加的选项可用，不过你只会在调查第七章节的内存泄漏时才用得到。<br>
</font> <p></p>
[11] See Section 7.2<br>
[12] See Section 7.3.2
<p></p> <font color="green">
[注11]：参见章节7.2<br>
[注12]：参见章节7.3.2<br>
</font> <p></p>

