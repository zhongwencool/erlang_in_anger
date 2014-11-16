# Too Many (or to o few) Processes
The process count is mostly useful when you know your node’s usual average count <sup>6</sup>, in
order to figure if it’s abnormal or not.<br>
&emsp;A count that is higher than normal may reveal a specific leak or overload, depending
on applications.<br>
If the process count is extremely low compared to usual, see if the node terminated with
a slogan like:<br>
<p></p> <font color="green">
&emsp;当你掌握节点正常时的平均进程数<sup>6</sup>时，你就可以通过进程数来判断目前的系统是不是正常。
&emsp;高于正常水平的进程数可能暗示着某种泄漏或超载，这取决于你的applicatons。<br>
&emsp;如果你的进程数远低于平均水平，那看看节点是否被终结于下面这些原因：<br>
</font> <p></p>

------------------------------------------------------<br>
`<br> Kernel pid terminated (application_controller)`<br>
`<br> ({application_terminated, <AppName>, shutdown})`<br>
------------------------------------------------------<br>
&emsp;In such a case, the issue is that a specific application (< AppName >) has reached its
maximal restart frequency within its supervisors, and that prompted the node to shut down. Error logs that led to the cascading failure should be combed over to figure things
out.
<p></p> <font color="green">
&emsp; 在上面这个例子中，问题在于某个(< AppName >)了的application已被supervisors重启的次数达到了规定的最大值，这暗示着节点将会停工。Error日志可以协助你把造成此连锁错误的问题找出来。
</font> <p></p>

[6] See subsection 5.1.3 for details<br>

<p></p> <font color="green">
[注6]: 更多细节看章节5.1.3
</font> <p></p>
