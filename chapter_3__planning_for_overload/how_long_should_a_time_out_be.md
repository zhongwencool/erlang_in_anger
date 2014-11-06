# How Long Should a Time Out Be
# timeout 应该设置为多少？
What’s particularly tricky about applying back-pressure to handle overload via synchronous calls is having to determine what the typical operation should be taking in terms of time, or rather, at what point the system should time out.
<p></p> <font color="green">

&emsp;尤其棘手的是通过同步调用处理负荷的负压式应用(applying back-pressure)：必须要给典型的操作定一个执行时间，更确定地说就是操作执行时间达到指定最大的timeout时间后，这个操作会无效.
</font> <p></p>

The best way to express the problem is that the first timer to be started will be at the edge of the system, but the critical operations will be happening deep within it.This means that the timer at the edge of the system will need to have a longer wait time that those within, unless you plan on having operations reported as timing out at the edge even though they succeeded internally.
<p></p> <font color="green">
&emsp;最好表述这一问题的方法就是：计时器开始于系统边缘(the edge of the system)，但关键的操作却发生在系统的深处。这意味着系统边缘的计时器将需要更长的等待时间。除非你打算即便他们成功了，也要加上系统边缘的超时。[神啊，来帮我翻译下这段吧！！]
(This means that the timer at the edge of the system will need to have a longer wait time that those within, unless you plan on having operations reported as timing out at the edge even though they succeeded internally.)
</font> <p></p>

An easy way out of this is to go for infinite timeouts. Pat Helland <sup>9</sup> has an interesting answer to this:<br>
Some application developers may push for no timeout and argue it is OK to wait indefinitely. I typically propose they set the timeout to 30 years.<br>
 That, in turn, generates a response that I need to be reasonable and not silly. Why is 30 years sily but infinity is reasonable? I have yet to see a messaging application
that really wants to wait for an unbounded period of time. . .
This is, ultimately, a case-by-case issue. In many cases, it may be more practical to use a different mechanism for that flow control. <sup>10</sup>
<p></p> <font color="green">
&emsp;一个简单的方法就是把timeout设置为infinite。对于这，PatHelland<sup>9</sup>有一个非常有意思的回答:<br>
&emsp;一些application开发者可能会推行不能有timeout并争论无限等待是OK的.我通常建议他们把timeout设置为30年。
反过来，就会得到质疑：我要一个合理而并不愚蠢答案，为什么30年就是愚蠢的而无限(infinity)就是合理的？<br>
我曾看到个一个无限等待的消息传递的application,最终，这只是case-by-case的问题，在很多case里面，如果使用不同的流程机制或许会更加符合实际<sup>10</sup>。
</font> <p></p>

[9] Idempotence is Not a Medical Condition, April 14, 2012
[10] In Erlang, using the value infinity will avoid creating a timer, avoiding some resources. If you do
use this, remember to at least have a well-defined timeout somewhere in the sequence of calls.
<p></p> <font color="green">

[注9]：Idempotence is Not a Medical Condition, April 14, 2012
[注10]: Erlang中使用infinity不会创建一个定时器，不会使用一些资源。如果你打算这样用，请不要忘记在一系列的call中有至少要有一个定义好的timeout时间。
</font> <p></p>
