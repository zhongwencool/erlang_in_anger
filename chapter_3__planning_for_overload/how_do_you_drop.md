# How Do You Drop
# 丢弃数据的原则
Most of the solutions outlined here work based on message quantity, but it’s also possible to try and do it based on message size, or expected complexity, if you can predict it. When using a queue or stack buffer, instead of counting entries, all you may need to do is count their size or assign them a given load as a limit.<br>
I’ve found that in practice, dropping without regard to the specifics of the message works rather well, but each application has its share of unique compromises that can be acceptable or not <sup>25</sup>.
<p></p> <font color="green">
&emsp;大部分这里列出的方法都是基于消息数量的，但也可以试下基于消息大小或你所能预知的更复杂的指标。当你使用队列或堆缓冲区(stack buffer)，而不是计算消息总数量，你可能需要做的是给程序规定好它的大小，或限制它们的负载。<br>
&emsp;我在实践中发现：丢弃时不去管消息本身的用途非常好用，但每个application都会自己独特地妥协机制来决定可不可以这样做<sup>25</sup>。
</font> <p></p>
There are also cases where the data is sent to you in a "fire and forget" manner — the entire system is part of an asynchronous pipeline — and it proves difficult to provide feedback to the end-user about why some requests were dropped or are missing.<br>
If you can reserve a special type of message that accumulates dropped responses and tells the user " N messages were dropped for reason X", that can, on its own, make the compromise far more acceptable to the user.<br>
This is the choice that was made with Heroku’s logplex log routing system, which can spit out L10 errors, alerting the user that a part of the system can’t deal with all the volume right now.<br>
In the end, what is acceptable or not to deal with overload tends to depend on the humans that use the system. It is often easier to bend the requirements a bit than develop new technology, but sometimes it is just not avoidable.
<p></p> <font color="green">
&emsp;还有一些情况是是数据发给你，然后就不管了的("fire and forget") ---那整个系统都是异步管道(asynchronous pipeline)的一部分----这个就很难给终端用户反馈为什么有些请求会被丢弃或丢失掉。<br>
&emsp;如果你可以规定一种特定的消息类型可能会被丢弃，并告诉用户"N条消息因某某原因被丢弃掉了",那么相比于什么都不反馈给用户来说，这就更能接受点。<br>
&emsp;HeroKu的logplex &emsp;就是这样为系统路由日志的，它可以把日志分成10个等级，并警告系统的那个等级的日志现在不能处理。
</font> <p></p>
<p></p> <font color="green">
&emsp;最后，什么是可接受的或不能处理的负载往往取决于使用这个系统的人。改一下需求可能会比开发一个新技术更加容易，但有时又是不可避免地(有的需要是不能改的)。
</font> <p></p>

[25] Old papers such as Hints for Computer System Designs by Butler W. Lampson recommend dropping
messages: "Shed load to control demand, rather than allowing the system to become overloaded." The
paper also mentions that "A system cannot be expected to function well if the demand for any resource
exceeds two-thirds of the capacity, unless the load can be characterized extremely well." adding that "The
only systems in which cleverness has worked are those with very well-known loads."
<p></p> <font color="green">
[注25] ： Butler W.Lampson写的 Hints for Computer System Designs 论文中建议:"宁愿控制负载，也不能让系统超负荷",论文还指出"一个系统如果被要求工作大于2/3的负载之上，就不可以被认为是正常工作的"，"聪明的系统都会工作在适当的负载下"。
</font> <p></p>
