# Restricting Input
# 限制输入
Restricting input is the simplest way to manage message queue growth in Erlang systems.<br>
It’s the simplest approach because it basically means you’re slowing the user down (applying back-pressure), which instantly fixes the problem without any further optimization required.<br>
On the other hand, it can lead to a really crappy experience for the user.
<p></p> <font color="green">
&emsp;限制输入是处理Erlang系统日益增长的消息队列中最简单的方法。<br>
&emsp;之所以简单，是因为这意味着减慢了用户(相对于满负荷:back-pressure工作来说)，这可以马上修复问题且不要求做更多的优化。<br>
&emsp;但另一方面，这会给用户一个非常糟糕的体验。
</font> <p></p>
The most common way to restrict data input is to make calls to a process whose queue would grow in uncontrollable ways synchronously. By requiring a response before moving on to the next request, you will generally ensure that the direct source of the problem will be slowed down.
<p></p> <font color="green">
&emsp;最常用的限制输入的方法就是使无法控制自己消息队列增长的进程变成同步处理。在处理下一个请求(request)之前需要前一个请求的回应(response),这样同步处理肯定会使效率慢下来。
</font> <p></p>
The difficult part for this approach is that the bottleneck causing the queue to grow is usually not at the edge of the system, but deep inside it, which you find after optimizing nearly everything that came before. Such bottlenecks will often be database operations, disk operations, or some service over the network.
<p></p> <font color="green">
&emsp;这个方法的难度在于造成消息队列增长的短板通常不是系统边缘(the edge of the system)导致的，当你深入理解后，你就会发现要去优化系统的每个细节。这个瓶颈通常来自于数据库操作，硬盘读写，或其它要借住网络的服务。
</font> <p></p>
This means that when you introduce synchronous behaviour deep in the system, you’ll possibly need to handle back-pressure, level by level, until you end up at the system’s edges and can tell the user, "please slow down."
<p></p> <font color="green">
&emsp;这就意味当你介绍系统的深度同步的行为时，你可能从系统内部到边缘一层层地限制负荷，并还要忠告用户：”请悠着点用“。
</font> <p></p>
Developers that see this pattern will often try to put API limits per user <sup>8</sup> on the system entry points. This is a valid approach, especially since it can guarantee a basic quality of service (QoS) to the system and allows one to allocate resources as fairly (or unfairly) as desired.
<p></p> <font color="green">
&emsp;开发者看到这种模式会经常试图在系统的入口的API给每个用户都做限制<sup>8</sup>。 这的确是一个有效的方法，特别是可以保证一个最基本的服务质量(base quality of service,QoS)。可以允许用户自由地分配资源。
</font> <p></p>
[8] There’s a tradeoff between slowing down all requests equally or using rate-limiting, both of which are valid. Rate-limiting per user would mean you’d still need to increase capacity or lower the limits of all users when more new users hammer your system, whereas a synchronous system that indiscriminately blocks
should adapt to any load with more ease, but possibly unfairly.<br>
<p></p> <font color="red">
[注8]：减缓所有请求和使用速度限制之间有一个权衡，两者可以结合使用。速度限制每个用户将意味着你仍然需要增加容量或放宽对新用户加放系统的限制，而对于同步系统来说,就是不加选择地放宽限制来适应任何负载,但可能存在不公平。
</font> <p></p>

