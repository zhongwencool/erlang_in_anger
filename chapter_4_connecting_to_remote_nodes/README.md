# Chapter 4 Connecting to Remote Nodes
# 连接到远程节点
Interacting with a running server program is traditionally done in one of two ways. One is to do it through an interactive shell kept available by using a screen or tmux session that runs in the background and letting someone connect to it.<br>
The other is to program management functions or comprehensive configuration files that can be dynamically reloaded.<br>
The interactive session approach is usually okay for software that runs in a strict ReadEval-Print-Loop (REPL). The programmed management and configuration approach requires careful planning in whatever tasks you think you’ll need to do, and hopefully getting it right.<br>
Pretty much all systems can try that approach, so I’ll skip it given I’m somewhat more interested in the cases where stuff is already bad and no function exists for it.
<p></p> <font color="green">
&emsp;与运行中服务器程序交互通常有两种方式。一种是使用一个通过屏幕的交互shell或tmux会话(在后台运行，让用户来主动连接它)来保持交互状态。<br>
&emsp;另一种就是通过程序管理功能或动态加载的综合配置文件。<br>
&emsp;使用交互会话(session)的方法通常都是运行在严格遵守ReadEval-Print-Loop(REPL)标准的机器上.程序管理和配置文件方法则需要仔细规划好你需要要做的每个任务，并希望把它们都弄清楚。<br>
&emsp;几乎所有的系统都可以尝试这种方法，所以我会跳过这部分，毕竟我对事情已变糟糕后，产生这种情况的原因更感兴趣。
</font> <p></p>
Erlang uses something closer to an "interactor" than a REPL. Basically, a regular Erlang virtual machine does not need a REPL, and will happily run byte code and stick with that, no shell needed.<br>
However, because of how it works with concurrency and multiprocessing, and good support for distribution, it is possible to have in-software REPLs that run as arbitrary Erlang processes.<br>
This means that, unlike a single screen session with a single shell, it’s possible to have as many Erlang shells connected and interacting with one virtual machine as you want at a time <sup>1</sup>.<br>
Most common usages will depend on a cookie being present on the two nodes you want to connect together<sup>2</sup>, but there are ways to do it that do not include it.<br>
Most usages will also require the use of named nodes, and all of them will require a priori measures to make sure you can contact the node.
<p></p> <font color="green">
&emsp;Erlang使用一个近似于交互器的东西(不是REPL). 基本上，一个正常的Erlang虚拟机根本不需要REPL,就能愉快地运行字节代码并一直保持会话，而且不需要shell.<br>
&emsp;但是，因为它要处理并发和多进程，并要支持分布式，这个需要就可能要使用REPL软件运行任意的Erlang进程。<br>
&emsp;这也就意味着，并不仅是使用一个屏幕的单个shell，可能同时会有很多Erlang shell与虚拟机相连接，来交互 <sup>1</sup>。<br>
&emsp;最常用的用法是依赖相同的cookie让2个节点连接在一起<sup>2</sup>，但也有其它方法可以做到这一点。<br>
&emsp;大多数方法都需要知道连接的节点名，并且都需要你先确定好能连接到此节点。
</font> <p></p>
[1] More details on the mechanisms at http://ferd.ca/repl-a-bit-more-and-less-than-that.html <br>
[2] More details at http://learnyousomeerlang.com/distribunomicon#cookies or http://www.erlang.org/doc/reference_manual/distributed.html#id83619

<p></p> <font color="green">
[注1] ：更多细节 http://ferd.ca/repl-a-bit-more-and-less-than-that.html <br>
[注2] ：更多细节 http://learnyousomeerlang.com/distribunomicon#cookies 或 http://www.erlang.org/doc/reference_manual/distributed.html#id83619
</font> <p></p>

