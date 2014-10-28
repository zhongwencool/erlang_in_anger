# Too Many Ports
Similarly to the process count, the port count is simple and mostly useful when you know
your usual values <sup>7</sup>.<br>
A high count may be the result of overload, Denial of Service attacks, or plain old
resource leaks. Looking at the type of port leaked (TCP, UDP, or files) can also help reveal if there was contention on specific resources, or if the code using them is just wrong.
<p></p> <font color="green">
&emsp;同进程数相类似，如果你知道系统正常时的平均端口数，那么端口数也是一个简单但非常有用的指标。
&emsp;如果端口数太多，可能是上越负荷，被攻击了(Denial of Service attacks),或普通的资源泄漏。可以通过查看端口泄露的类型(TCP,UDP,files)来判断是否有某个资源被多个进程所竞争。或者是只是用到他们的代码错了。<br>
</font> <p></p>

[7] See subsection 5.1.4 for details
<p></p> <font color="green">
[注7]：更多细节见章节5.1.4<br>
</font> <p></p>
