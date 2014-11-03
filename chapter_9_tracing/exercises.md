# Exercises

## Review Questions
[1]. Why is debugger use generally limited on Erlang?<br>
[2]. What are the options you can use to trace OTP processes?<br>
[3]. What determines whether a given set of functions or processes get traced?<br>
[4]. How can you stop tracing with recon_trace? With other tools?<br>
[5]. How can you trace non-exported function calls?<br>
<p></p> <font color="green">
[1] 为什么debugger在Erlang使用通常会受限？<br>
[2] 如果trace OTP进程，你可以使用什么选项？<br>
[3] 决定trace给定的函数还是trace进程的因素是什么？<br>
[4] 使用recon_trace怎么停止trace?其它工具呢？<br>
[5] 如果trace没有导出的函数？<br>
</font> <p></p>

## Open-ended Questions
[1]. When would you want to move time stamping of traces to the VM’s trace mechanisms directly? What would be a possible downside of doing this?<br>
[2]. Imagine that traffic sent out of a node does so over SSL, over a multi-tenant system.
However, due to wanting to validate data sent (following a customer complain), you need to be able to inspect what was seen clear text. Can you think up a plan to be able to snoop in the data sent to their end through the ssl socket, without snooping on the data sent to any other customer?<br>
<p></p> <font color="green">
[1] 什么情况下，你会把时间戳直接加入到VM的trace机制？这样做可能带来的负面影响将是什么？<br>
[2] 假设在一个多用户的系统中，节点间的通信是使用SSL的。由于想要验证数据发送(客户抱怨后),你不得不检查明文内容。请想出一个方案：可以通过SSL在数据的发送端窥探数据(把数据发给客户前验证数据)。<br>
</font> <p></p>

## Hands-On
Using the code at https://github.com/ferd/recon_demo (these may require a decent understanding of the code there):<br>
[1]. Can chatty processes (council_member) message themselves? (hint: can this work
with registered names? Do you need to check the chattiest process and see if it messages
itself ?)<br>
[2]. Can you estimate the overall frequency at which messages are sent globally?<br>
[3]. Can you crash a node using any of the tracing tools? (hint: dbg makes it easier due
to its greater flexibility)<br>
<p></p> <font color="green">
&emsp;使用这里的代码：https://github.com/ferd/recon_demo (这些可能需要对代码做深入理解)：<br>
[1] 请详细说说进程(council_member)间是发消息机制。(提示：可以用注册进程的机制么？你需要检查最忙的进程来看看他的消息是怎样的么？)<br>
[2] 你能估计下全局发消息总体频率么？<br>
[3] 你可以使用哪一种trace工具把节点弄崩溃么？(提示：因为dbg有更大的灵活性，所以它会更容易些，)<br>
</font> <p></p>

