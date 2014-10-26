# Chapter 5 Runtime Metrics
# 运行时指标(Runtime Metrics)
One of the best selling points of the Erlang VM for production use is how transparent it can be for all kinds of introspection, debugging, profiling, and analysis at run time.<br>
The advantage of having these runtime metrics accessible programmatically is that building tools relying on them is easy, and building automation for some tasks or watchdogs is equally simple <sup>1</sup>. <br>
Then, in times of need, it’s also possible to bypass the tools and go direct to the VM for information.
<p></p> <font color="green">
Erlang 虚拟机生产使用是透明是Erlang最好的卖点之一：可以进行各种各样的内部查看，在运行时调试和分析。<br>
这些可供运行时以程序方式访问的指标优点在于：用它们构建工具非常简单，为某些任务构建自动化或监管机制(watchdogs)也非常简单<sup>1</sup>。<br>
然后，再需要的时间，就可以使用工具直接查看VM的相关信息。
<p></p>
A practical approach to growing a system and keeping it healthy in production is to make sure all angles are observable: in the large, and in the small. There’s no generic recipe to tell in advance what is going to be normal or not.<br>
You want to keep a lot of data and to look at it from time to time to form an idea about what your system looks like under normal circumstances. The day something goes awry, you will have all these angles you’ve grown to know, and it will be simpler to find what is off and needs fixing.
<p></p> <font color="green">
保持系统日益增长且不出毛病的一个实用方法就是确保可以检测到无论大小的指标。这里没有通用的方法可以提前告之你接下来是否正常。<br>
你想不断地查看保存好的大量的数据来，判断你的系统在正常情况下是什么样子。等到出岔子时，你可以知道你想知道的一切，然后就可以轻松地找到什么出错了并修复它。
</font> <p></p>
For this chapter (and most of those that follow), most of the concepts or features to be shown are accessible through code in the standard library, part of the regular OTP distribution.<br>
However, these features aren’t all in one place, and can make it too easy to shoot yourself in the foot within a production system. They also tend to be closer to building blocks than usable tools.
<p></p> <font color="green">
这一章节(还有接下来的大部分章节)，大部分的概念或特性都可以通过标准库中的代码看到，一部分是常用OTP分布(OTP distuibution)。<br>
但这些特性并不是都在同一个地方，并且非常容易让你在生产系统中搬石头砸自己的脚。相比那些可用工具(usable tools)来说，他们更接近于构建块(building blocks)工具.
</font> <p></p>
Therefore, to make the text lighter and to be more usable, common operations have been regrouped in the recon <sup>2</sup> library, and are generally production-safe.
<p></p> <font color="green">
因此，为了使文本更加轻，更有用，常用的操作都被放在了recon<sup>2</sup>库里，他们都是可以放心使用的。
</font> <p></p>
[1] Making sure your automated processes don’t run away and go overboard with whatever corrective actions they take is more complex<br>
[2] http://ferd.github.io/recon/


<p></p> <font color="green">

[注1]：确保你的自动华过程不走极端，不跑偏。<br>
[注2]：http://ferd.github.io/recon/。
</font> <p></p>
