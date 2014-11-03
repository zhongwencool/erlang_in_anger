# Chapter 9 Tracing
One of the lesser known and absolutely under-used features of Erlang and the BEAM virtual
machine is just about how much tracing you can do on there.
<br>&emsp;Forget your debuggers, their use is too limited. <sup>1</sup> Tracing makes sense in Erlang at all steps of your system’s life cycle, whether it’s for development or for diagnosing a running production system.
<p></p> <font color="green">
&emsp;Erlang有一个不太为人知的但绝对未充分利用的特性，BEAM虚拟机可以让你尽情地trace。<br>
&emsp;扔掉你的debuggers吧，他们的使用时限制太多了<sup>1</sup>。Erlang trace的意义在于你可以trace你系统生命周期的所有步骤，不论是为了开发，还是为了诊断生产环境下运行的系统。
</font> <p></p>

<br>&emsp;There are a few options available to trace Erlang programs:
<br>&emsp;• **sys** <sup>2</sup> comes standard with OTP and allows to set custom tracing functions, log all
kinds of events, and so on. It’s generally complete and fine to use for development. It
suffers a bit in production because it doesn’t redirect IO to remote shells, and doesn’t
have rate-limiting capabilities for trace messages. It is still recommended to read the
documentation for the module.
<br>&emsp;• **dbg** <sup>3</sup> also comes standard with Erlang/OTP. Its interface is a bit clunky in terms of
usability, but it’s entirely good enough to do what you need. The problem with it is
that you have to know what you’re doing, because dbg can log absolutely everything
on the node and kill one in under two seconds.
<br>&emsp;• *tracing **BIF**s* are available as part of the erlang module. They’re mostly the raw blocks used by all the applications mentioned in this list, but their lower level of abstraction makes them rather difficult to use.
<br>&emsp;• **redbug** <sup>4</sup> is a production-safe tracing library, part of the **eper** <sup>5</sup> suite. It has an internal rate-limiter, and a nice usable interface. To use it, you must however be willing to add in all of eper’s dependencies. The toolkit is fairly comprehensive and can be a very interesting install.
<br>&emsp;• **recon_trace** <sup>6</sup> is recon’s take on tracing. The objective was to allow the same levels of safety as with redbug, but without the dependencies. The interface is different, and the rate-limiting options aren’t entirely identical. It can also only trace function calls, and not messages. <sup>7</sup>
<br>&emsp;This chapter will focus on tracing with **recon_trace**, but the terminology and the concepts used mostly carry over to any other Erlang tracing tool that can be used.
<p></p> <font color="green">
&emsp;下面介绍几个trace Erlang进程利器：<br>
&emsp;• **sys** <sup>2</sup>是一个标准的OTP，可以允许自定义trace函数，记录所有类型的事件等等。它非常完善且可以很好地用于开发。但它会稍微影响处于生产环境的系统，因为它没有把IO重定向到远程的shell中，而且他没有限制trace消息的速度。不过还是推荐阅读的文档模块。<br>
&emsp;• **dbg** <sup>3</sup>也是一个标准的OTP。它的接口在可用性方面显得有点笨拙。但它完全足以满足你所需。问题在于：你必须要知道你要做什么,因为dbg可以记录一切，并在2秒把系统搞崩溃。<br>
&emsp;• *tracing **BIF**s*作为一个Erang的模块可用。它们大多作为原始块(the raw blocks)由这个列表中提到的application所调用,但由于他们处于较底层，比较抽象，用起来也非常困难。<br>
&emsp;• **redbug** <sup>4</sup>是可以在正式的生产运行系统中使用的trace库，是**eper** <sup>5</sup>的一部分，它有一个内部的速度限制开关，和一个不错的可用接口。为了使用它，你必须把eper的所有依赖项都加上。这个工具箱非常全面，你会有一个非常有趣的安装。<br>
&emsp;• **recon_trace** <sup>6</sup>是recon中负责trace的模块。目的是和redbug有相同的安全水平,但却不要这么多的依赖项。接口也不一样，速度限制选项并不完全相同。它可以只trace指定的函数调用，不是针对消息的那种 <sup>7</sup>。<br>
&emsp;本章节会集中于使用**recon_trace**来trace，但这里面使用的术语和概念在其它的Erlang trace工具里面也被用到。<br>
</font> <p></p>

[1] One common issue with debuggers that let you insert break points and step through a program is that they are incompatible with many Erlang programs: put a break point in one process and the ones around keep going. In practice, this turns debugging into a very limited activity because as soon as a process needs to interact with the one you’re debugging, its calls start timing out and crashing, possibly taking down the entire node with it. Tracing, on the other hand, doesn’t interfere with program execution, but still gives you all the data you need.<br>
[2] http://www.erlang.org/doc/man/sys.html<br>
[3] http://www.erlang.org/doc/man/dbg.html<br>
[4] https://github.com/massemanet/eper/blob/master/doc/redbug.txt<br>
[5] https://github.com/massemanet/eper<br>
[6] http://ferd.github.io/recon/recon_trace.html<br>
[7] Messages may be supported in future iterations of the library. In practice, the author hasn’t found the need when using OTP, given behaviours and matching on specific arguments allows to get something roughly equivalent.<br>
<p></p> <font color="green">
[注1]：debuggers有一个常见的问题：让你插入断点然后一步步执行程序，这对很多erlang进程到说都是不兼容的：把在一个进程中放入断点，与它相关的却还在进行。在实践时，这让debug变得非常局限，因为进程一旦需要与你打断点的进程交互，这个进程就会timeout然后崩溃掉，这就有可能引整个节点连锁反应然后挂掉。而tracing则不会影响程序的执行，但却可以得到你想要的所有数据哦。<br>
[注2]：http://www.erlang.org/doc/man/sys.html<br>
[注3]：http://www.erlang.org/doc/man/dbg.html<br>
[注4]：https://github.com/massemanet/eper/blob/master/doc/redbug.txt<br>
[注5]：https://github.com/massemanet/eper<br>
[注6]：http://ferd.github.io/recon/recon_trace.html<br>
[注7]：消息可能在将来的版本有支持库。在实践中，鉴于行为和匹配特定的参数大致相同，作者认为当使用OTP时根本没有必要支持消息trace的特性。
</font> <p></p>



