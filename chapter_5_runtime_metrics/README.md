# Chapter 5 Runtime Metrics
# 运行时指标(Runtime Metrics)
One of the best selling points of the Erlang VM for production use is how transparent it can be for all kinds of introspection, debugging, profiling, and analysis at run time.<br>
&emsp;The advantage of having these runtime metrics accessible programmatically is that building tools relying on them is easy, and building automation for some tasks or watchdogs is equally simple <sup>1</sup>. <br>
&emsp;Then, in times of need, it’s also possible to bypass the tools and go direct to the VM for information.
<p></p> <font color="green">
Erlang最好的卖点之一就是VM在生产过程中是完全透明：可以进行各式各样的内部查看，在运行时调试和分析。<br>
&emsp;这些可在运行时访问程序各项指标优点在于：用它们构建工具非常简单，为构建自动化的某些任务或监管机制(watchdogs)也非常简单<sup>1</sup>。<br>
&emsp;然后，万一有事时，就可以使用工具直接查看VM的相关信息。
<</font> <p></p>
&emsp;A practical approach to growing a system and keeping it healthy in production is to make sure all angles are observable: in the large, and in the small. There’s no generic recipe to tell in advance what is going to be normal or not.<br>
&emsp;You want to keep a lot of data and to look at it from time to time to form an idea about what your system looks like under normal circumstances. The day something goes awry, you will have all these angles you’ve grown to know, and it will be simpler to find what is off and needs fixing.<br>
&emsp;For this chapter (and most of those that follow), most of the concepts or features to be shown are accessible through code in the standard library, part of the regular OTP distribution.<br>
&emsp;However, these features aren’t all in one place, and can make it too easy to shoot yourself in the foot within a production system. They also tend to be closer to building blocks than usable tools.<br>
&emsp;Therefore, to make the text lighter and to be more usable, common operations have been regrouped in the recon <sup>2</sup> library, and are generally production-safe.
<p></p> <font color="green">
&emsp;确保可以检测到所有的指标是保持日益壮长的系统不出毛病的一个实用方法。这里没有通用的办法可以提前告之你:接下来的情况是否正常。<br>
&emsp;你可以通过查看对比正常状态下储存好的大量指标，判定你的系统在正常情况下是什么样子。等到出岔子时，你已经全方位地了解了系统，然后就可以轻松地找到什么出错了并修复它。<br>
&emsp;这一章节(还有接下来的大部分章节)，大部分的概念或特性都可以通过标准库（regular OTP distuibution的一部分）中的代码看到。<br>
&emsp;但这些特性并不是都出现在同一个地方，还非常容易让你在生产系统中搬石头砸自己的脚。相比那些可用工具(usable tools)来说，他们更接近于构建块(building blocks)工具.<br>
&emsp;因此，为了使文本更易于阅读，更有用，常用的操作都被放在了recon<sup>2</sup>库里，他们都是可以放心使用的。
</font> <p></p>
[1] Making sure your automated processes don’t run away and go overboard with whatever corrective actions they take is more complex<br>
[2] http://ferd.github.io/recon/


<p></p> <font color="green">

[注1]：不管他们采取纠正行动有多复杂，也要确保你的自动化过程不走极端，不跑偏。<br>
[注2]：http://ferd.github.io/recon/。
</font> <p></p>
