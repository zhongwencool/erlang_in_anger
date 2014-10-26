# Digging In
# 深入理解
Whenever some ’in the large’ view (or logging, maybe) has pointed you towards a potential
cause for an issue you’re having, it starts being interesting to dig around with a purpose. Is a process in a weird state? Maybe it needs tracing <sup>16</sup>! Tracing is great whenever you have a specific function call or input or output to watch for, but often, before getting there, a lot more digging is required.<br>
Outside of memory leaks, which often need their own specific techniques and are discussed in Chapter 7, the most common tasks are related to processes, and ports (file descriptors and sockets).
<p></p> <font color="green">
&emsp;当某些全局的指标(或日志logging)指明你遇见问题的可能原因时，就可以开始有目的的去挖掘他的本质原因是什么啦。有进程处于一个奇怪的状态？可以需要tracing<sup>16</sup>这个进程！tracing非常棒，它可以追踪任意一个具体的函数调用时的输入或输出，但在此之前，我们还需要了解一些关于挖掘(digging)所必需了解的东西。
</font> <p></p>



[16] See Chapter 9<br>

<p></p> <font color="green">
[16] 详见 章节9<br>
</font> <p></p>

