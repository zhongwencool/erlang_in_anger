# Side Effects
Of course, the libraries and processes that call such a client will then error out if they don’t expect to work without a database.
<p></p> <font color="green">
当然，如果他们期望工作状态是有数据库的，那么这个客户端上的进程就会出错。(the libraries and processes that call such a client will then error out if they don’t expect to work without a database. )
</font> <p></p>

That’s an entirely different issue in a different problem space, one that depends on your business rules and what you can or can’t do to a client, but one that is possible to work around.
<p></p> <font color="green">

这是2个在不同角度下，完全不同的问题，一个是取决于你的业务需求：客户端什么能做，什么不能做;另一个则是有没有可能正常工作。
</font> <p></p>

 For example, consider a client for a service that stores operational metrics — the code that calls that client could very well ignore the errors without adverse effects to the system as a whole.<br>
 The difference in both initialization and supervision approaches is that the client’s callers make the decision about how much failure they can tolerate, not the client itself.
<p></p> <font color="green">

比如，考虑到一个存储运营指标(编：应该是指服务器运行时的各种指标)(operational metrics)的客户端服务---客户端调用的代码就很可能忽略对系统产生不良影响的错误。<br>

在初始化和监控方法的不同之处：客户端的使用者来决定他们能容忍什么程序的错误，而不是客户端自己本身决定的。
</font> <p></p>

That’s a very important distinction when it comes to designing fault-tolerant systems.<br>
Yes, supervisors are about restarts, but they should be about restarts to a stable known state.
<p></p> <font color="green">

在设计容错系统中，这有非常大的区别。<br>
总之,supervisor是可以重启进程，但他们应当重启到一个稳定的已知状态。
</font> <p></p>
