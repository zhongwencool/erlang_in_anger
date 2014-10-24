# What Users See
# 用户会看到什么
The tricky part about back-pressure is reporting it. When back-pressure is done implicitly through synchronous calls, the only way to know it is at work due to overload is that the system becomes slower and less usable.
<p></p> <font color="green">
back-pressure棘手的部分就是如何报告它。当back_pressure隐密地通过同步调用完成时，知道它(存在)的唯一方法就是：超负荷让系统变得缓慢而不可用。
</font> <p></p>
Sadly, this is also going to be a potential symptom of bad hardware, bad network, unrelated overload, and possibly a slow client.<br>
Trying to figure out that a system is applying back-pressure by measuring its responsiveness is equivalent to trying to diagnose which illness someone has by observing that person has a fever.<br>
It tells you something is wrong, but not what.
<p></p> <font color="green">
悲剧的是，这也是硬件出错，网络不好，与过载无关，甚至也可能由于一个变慢的客户端的潜在症状。<br>
通过测量系统的响应来试图找出back-pressure就相当于通过观察一个人发烧来诊断他到底得了什么病一样。<br>
它只是告诉你某些东西出错了，但不会告诉你是什么出错了。
</font> <p></p>
Asking for permission, as a mechanism, will generally allow you to define your interface in such a way that you can explicitly report what is going on: the system as a whole is overloaded, or you’re hitting a limit into the rate at which you can perform an operation and adjust accordingly.
<p></p> <font color="green">
把请求许可作为一种机制，通常允许你定义一些可以显示报告到底发生了什么的接口：系统全面超负荷，还是你达到了某个极限速度。这样，你就可以根据结果作相应的调整。
</font> <p></p>
There is a choice to be made when designing the system. Are your users going to have per-account limits, or are the limits going to be global to the system?<br>
System-global or node-global limits are usually easy to implement, but will have the downside that they may be unfair. A user doing 90% of all your requests may end up
making the platform unusable for the vast majority of the other users.
<p></p> <font color="green">
还有一个设计系统时要考虑的一个因素。你的用户每次限制，还是只在系统全局做个限制？<br>
在系统层面或节点层面上做全局限制是非常容易的，但也有个缺点，就是不公平。其中一个用户请求占总数的90%，可能会导致其它用户完全不能使用这个平台了。
</font> <p></p>

Per-account limits, on the other hand, tend to be very fair, and allow fancy schemes such as having premium users who can go above the usual limits. This is extremely nice, but has the downside that the more users use your system, the higher the effective global system limit tends to move. Starting with 100 users that can do 100 requests a minute gives you a global 10000 requests per minute.<br>
Add 20 new users with that same rate allowed, and suddenly you may crash a lot more often.
<p></p> <font color="green">
而另一方面，如果对每个用户都作限制，就会非常公平，可以容许高级用户直接突破规则，这真的非常好，但这也会降低更多的用户使用你的系统，相比而言，全局限制的效率就更高一些，如果100个用户可以使用100请求/min，就相当于10000 请求/min的全局限制效果。<br>
再在相同条件下新加20个新用户，你就有可能经常崩溃。
</font> <p></p>
It’s important to consider the tradeoffs your business can tolerate from that point of view, because users will tend not to appreciate seeing their allowed usage go down all the time, possibly even more so than seeing the system go down entirely from time to time.
<p></p> <font color="green">
权衡一下你的业务是不是可以容忍这点非常重要，因为用户并不希望他们被限制，甚至更情愿系统偶尔整体慢下来。
</font> <p></p>

