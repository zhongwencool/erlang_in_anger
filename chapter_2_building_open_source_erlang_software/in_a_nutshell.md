# In a nutshell
## 总结
Production systems I have worked with have been a mix of both approaches.<br>
Things like configuration files, access to the file system (say for logging purposes), local resources that can be depended on (opening UDP ports for logs), restoring a stable state from disk or network, and so on, are things I’ll put into requirements of a supervisor and may decide to synchronously load no matter how long it takes (some applications may just end up having over 10 minute boot times in rare cases, but that’s okay because we’re possibly syncing gigabytes that we need to work with as a base state if we don’t want to serve incorrect information.)<br>
On the other hand, code that depends on non-local databases and external services will adopt partial startups with quicker supervision tree booting because if the failure is expected to happen often during regular operations, then there’s no difference between now
and later.<br>
 You have to handle it the same, and for these parts of the system, far less strict guarantees are often the better solution.
<p></p> <font color="green">

&emsp;我所在工作过的产品级系统里，充斥了这两种方法（supervisor启动中的同步和异步方法）的混合。<br>
比如：读写配置文件，为写日志进入文件系统,为写日志打开本地UDP端口资源，从磁盘或网络恢复正常状态等等这些工作，我都会把他们放到supervisor下，然后再决定是否同步加载(synchronously load)，在极少的情况下applications可能会有超过10分钟的启动时间，但这是ok的，因为如果我们不想提供错误的信息，就可能会需要同步GB级的数据来保证这个基本的状态。<br>
&emsp;另一方面，依赖非本地数据库的代码和对应的外部服务，会采用更快异步的监控树启动(不在初始化中做可能会失败的重连操作)，因为如果在正常操作下失败也经常发生，现在启动还是晚点再启动就没有区别。<br>
&emsp;你终究是要处理它（上面提到在正常操作下发生的失败），对于这部分的系统来说，通常更好的解决方案是不做严格的限制(far less strict guarantees)。
</font>
<p></p>
