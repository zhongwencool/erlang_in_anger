# Example Sessions
First let’s trace the **queue:new** functions in any process:<br>
<p></p> <font color="green">
&emsp;首先让我们trace 调用**queue:new**的任意的进程。
</font> <p></p>

----------------------------------------------------------<br>
`1> recon_trace:calls({queue, new, ’_’}, 1).`<br>
`1`<br>
`13:14:34.086078 <0.44.0> queue:new()`<br>
`Recon tracer rate limit tripped.`<br>
----------------------------------------------------------<br>

&emsp;The limit was set to 1 trace message at most, and recon let us know when that limit
was reached.<br>
&emsp;Let’s instead look for all the **queue:in/2** calls, to see what it is we’re inserting in queues:<br>
<p></p> <font color="green">
&emsp;把trace消息的限制最大为1，来试试recon通知我们已到最大限制的情况。<br>
&emsp;接着再调用**queue:in/2**，查看下我们什么消息被塞进了队列：<br>
</font> <p></p>

----------------------------------------------------------<br>
`2> recon_trace:calls({queue, in, 2}, 1).`<br>
`1`<br>
`13:14:55.365157 <0.44.0> queue:in(a, {[],[]})`<br>
`Recon tracer rate limit tripped.`<br>
----------------------------------------------------------<br>
<br>&emsp;In order to see the content we want, we should change the trace patterns to use a fun
that matches on all arguments in a list (_) and returns return_trace() . This last part
will generate a second trace for each call that includes the return value:<br>
<p></p> <font color="green">
&emsp;为了能看到我们想看到内容，我们应该把trace patterns转变为使用一个函数来匹配所有的参数列表(_)并返回return_trace()。最后一部分会为每次调用(包括返回值)都生成一秒的trace(a secnod trace)。<br>
</font> <p></p>
----------------------------------------------------------<br>

`3> recon_trace:calls({queue, in, fun(_) -> return_trace() end}, 3).`<br>
`1`<br>
`13:15:27.655132 <0.44.0> queue:in(a, {[],[]})`<br>
`13:15:27.655467 <0.44.0> queue:in/2 --> {[a],[]}`<br>
`13:15:27.757921 <0.44.0> queue:in(a, {[],[]})`<br>
`Recon tracer rate limit tripped.`<br>
----------------------------------------------------------<br>
<br>&emsp;Matching on argument lists can be done in a more complex manner:<br>
<p></p> <font color="green">
&emsp;匹配参数列表可以通过更复杂的方式完成:
</font> <p></p>

----------------------------------------------------------<br>
`4> recon_trace:calls(`<br>
`4> {queue, ’_’,`<br>
`4> fun([A,_]) when is_list(A); is_integer(A) andalso A > 1 ->`<br>
`4> return_trace()`<br>
`4> end},`<br>
`4> {10,100}`<br>
`4> ).`<br>
`32`<br>
`13:24:21.324309 <0.38.0> queue:in(3, {[],[]})`<br>
`13:24:21.371473 <0.38.0> queue:in/2 --> {[3],[]}`<br>
`13:25:14.694865 <0.53.0> queue:split(4, {[10,9,8,7],[1,2,3,4,5,6]})`<br>
`13:25:14.695194 <0.53.0> queue:split/2 --> {{[4,3,2],[1]},{[10,9,8,7],[5,6]}}`<br>
`5> recon_trace:clear().`<br>
`ok`<br>
----------------------------------------------------------<br>
<br>&emsp;Note that in the pattern above, no specific function (’_’ ) was matched against. Instead,
the fun used restricted functions to those having two arguments, the first of which is either
a list or an integer greater than 1.
<br>&emsp;Be aware that extremely broad patterns with lax rate-limitting (or very high absolute
limits) may impact your node’s stability in ways recon_trace cannot easily help you with.
Similarly, tracing extremely large amounts of function calls (all of them, or all of io for
example) can be risky if more trace messages are generated than any process on the node
could ever handle, despite the precautions taken by the library.
<br>&emsp;In doubt, start with the most restrictive tracing possible, with low limits, and progressively increase your scope.
<p></p> <font color="green">
&emsp;注意上面的pattern里，并没有特定的函数（’_’ ）匹配。取而代之的是，使用fun来限制函数：只能有两个参数，第一个是一个列表或一个比1大的整数。<br>
&emsp;请注意，非常宽泛的patterns与宽松的速率限制（或非常高的绝对限制）可能会影响你的节点稳定性，这样recon_trace就很难再次帮助你。<br>
&emsp;同理，trace非常多的函数调用(比如所有的进程，或所有的IO)都是非常危险：最后会导致节点上没有进程可以处理这么多的trace消息(尽管库函数已采取了预防措施)。<br>
&emsp;所以，带着疑问，先从最严格的trace开始，并一步步验证并扩大你的范围。<br>
</font> <p></p>

