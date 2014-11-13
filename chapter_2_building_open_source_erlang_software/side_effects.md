# Side Effects
Of course, the libraries and processes that call such a client will then error out if they don’t expect to work without a database.<br>
That’s an entirely different issue in a different problem space, one that depends on your business rules and what you can or can’t do to a client, but one that is possible to work around.<br>
 For example, consider a client for a service that stores operational metrics — the code that calls that client could very well ignore the errors without adverse effects to the system as a whole.<br>
 The difference in both initialization and supervision approaches is that the client’s callers make the decision about how much failure they can tolerate, not the client itself.<br>
 That’s a very important distinction when it comes to designing fault-tolerant systems.<br>
Yes, supervisors are about restarts, but they should be about restarts to a stable known state.
<p></p> <font color="green">
&emsp;当然,如果Client的类库或进程不希望在数据库宕机的情况还运行，那就报错吧。<br>
&emsp;不同的问题域有迥然不同的处理方式,这取决于业务逻辑和你要求client能做哪类处理和不能做那类处理的需求，说不定换成另一种情况，系统就就可以继续运行下去。<br>
&emsp;比如，考虑到一个存储众多运维指标(operational metrics)的client---client调用的代码就很可能忽略对整个系统产生不良影响的那些错误(因为正常情况下不会发现这种错误，对分析正常case没有帮助，并会扰乱正常情况)。<br>
&emsp;initialization与supervision方法的不同之处：在initialization过程中，client的使用者来决定他们能容忍什么程序的错误，而不是client自己本身决定的。<br>
&emsp;在设计容错系统中，这两者区别尤其重要。<br>
&emsp;总之,supervisor是可以重启进程，但他们应当重启到一个已知的稳定状态。
</font> <p></p>
