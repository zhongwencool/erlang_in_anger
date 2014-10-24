# Exercises
# 练习
 **Review Questions**<br>
 **复习问题：**<br>
[1]. Name the common sources of overload in Erlang systems<br>
[2]. What are the two main classes of strategies to handle overload?<br>
[3]. How can long-running operations be made safer?<br>
[4]. When going synchronous, how should timeouts be chosen?<br>
[5]. What is an alternative to having timeouts?<br>
[6]. When would you pick a queue buffer before a stack buffer?<br>
<p></p> <font color="green">

[1]. 列出Erlang系统中常见的过载源。<br>
[2]. 处理过载2个最基本的策略是什么。<br>
[3]. 怎样确保一个费时的操作安全？<br>
[4]. 当使用同步机制时，规定timeout的原则是？<br>
[5]. 还有什么方法可以替代设置timeout？<br>
[6]. 什么情况下优先使用队列缓冲，而不是堆栈缓冲？<br>
</font> <p></p>

** Open-ended Questions**<br>
** 开放式问题**<br>
[1]. What is a true bottleneck? How can you find it?<br>
[2]. In an application that calls a third party API, response times vary by a lot depending on how healthy the other servers are. How could one design the system to prevent occasionally slow requests from blocking other concurrent calls to the same service?<br>
[3]. What’s likely to happen to new requests to an overloaded latency-sensitive service where data has backed up in a stack buffer? What about old requests?<br>
[4]. Explain how you could turn a load-shedding overload mechanism into one that can also provide back-pressure.<br>
[5]. Explain how you could turn a back-pressure mechanism into a load-shedding mechanism.
[6]. What are the risks, for a user, when dropping or blocking a request? How can we prevent duplicate messages or missed ones?<br>
[7]. What can you expect to happen to your API design if you forget to deal with overload, and suddenly need to add back-pressure or load-shedding to it?<br>
<p></p> <font color="green">

[1].什么才是真正的瓶颈，你怎么去发现它？<br>
[2].在一个调用第三方API的应用程序中，响应时间通常都是依赖与别的服务器是不是正常。怎样设计系统来避免有时请求会变慢？<br>
[3]. 当一个数据备份在堆栈缓冲区的低延迟服务过载时，一个新的请求再来时，会发生什么？旧的请求又会怎样？<br>
[4]. 解释一下:你怎么把一个限制过载机制加到系统中，让它可以支持back-pressure.<br>
[5]. 解释一下:你怎么把一个back-pressure机制加入一个限制过载的系统中.<br>
[6]. 当丢弃或阻塞请求时，对用户有什么风险？怎样才能防止重复消息或错过消息？<br>
[7]. 如果你忘记处理过载了，你在设计API时会还要做什么？实然需要加入back-pressure或load-shedding？
</font> <p></p>

