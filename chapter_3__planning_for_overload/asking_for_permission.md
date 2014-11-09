# Asking For Permission
# 请求许可
A somewhat simpler approach to back-pressure is to identify the resources we want to block on, those that cannot be made faster and are critical to your business and users. Lock these resources behind a module or procedure where a caller must ask for the right to make a request and use them.<br>
There’s plenty of variables that can be used: memory, CPU, overall load, a bounded number of calls, concurrency, response times, a combination of them, and so on.
<p></p> <font color="green">
&emsp;back-pressure有一些简单的方法：把那些不能变快和对你业务和用户来说至关重要的资源，都打上标识，如果一个模块或程序想调用这个资源，就必须先申请到使用此资源的权限才能使用它。<br>
&emsp;这里有很多参考值可以作为衡量标准：内存，CPU,总负载，达到一定量的调用，并发活动，应答时间以及一系列他们的组合等等。
</font> <p></p>
The SafetyValve <sup>11</sup> application is a system-wide framework that can be used when you know back-pressure is what you’ll need.<br>
For more specific use cases having to do with service or system failures, there are plenty of circuit breaker applications available. Examples include breaky <sup>12</sup>, fuse <sup>13</sup>, or Klarna’s circuit_breaker <sup>14</sup>.<br>
Otherwise, ad-hoc solutions can be written using processes, ETS, or any other tool available.
<p></p> <font color="green">
&emsp;安全阀值(SafetyValve) application就是一个可以让你得到你想了解的back-pressure指标的系统级的框架。<br>
&emsp;下面还很多关于这方面的应用，你可以找到更具体的用例与服务或系统故障，比如：breaky<sup>12</sup>， fuse<sup>13</sup>， Klarna’s<sup>14</sup> circuit_breaker<br>
&emsp;另外，主流的解决方案是使用进程,ETS,或其它可用的工具。
</font> <p></p>
The important part is that the edge of the system (or subsystem) may block and ask for the right to process data, but the critical bottleneck in code is the one to determine whether that right can be granted or not.<br>
The advantage of proceeding that way is that you may just avoid all the tricky stuff about timers and making every single layer of abstraction synchronous.<br>
You’ll instead put guards at the bottleneck and at a given edge or control point, and everything in between can be expressed in the most readable way possible.
<p></p> <font color="green">
&emsp;系统边缘(the edge of the system)或子系统中重要的部分可能会阻塞并请求处理数据，但是代码中关键的瓶颈在于什么情况下可以授权？<br>
&emsp;这样处理的优势在于：你可以禁止所有棘手的时间定时器和让每一个抽象层(single layer of abstraction)都同步.<br>
&emsp;相反，你会在瓶颈处设置好防护，设置好一个阀值，在此范围内的所有都可以被描清楚。
</font> <p></p>
[11] https://github.com/jlouis/safetyvalve<br>
[12] https://github.com/mmzeeman/breaky<br>
[13] https://github.com/jlouis/fuse<br>
[14] https://github.com/klarna/circuit_breaker<br>
<p></p> <font color="green">
[注11]：1https://github.com/jlouis/safetyvalve .<br>
[注12]：2https://github.com/mmzeeman/breaky <br>
[注13]：https://github.com/jlouis/fuse<br>
[注14]：https://github.com/klarna/circuit_breaker
</font> <p></p>


