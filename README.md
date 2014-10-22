


<< Stuff Goes Bad: Erlang in Anger >> 由 Fred Hébert(写Learn Your Some Erlang的帅小伙) 和 Heroku 联合推出，遵循 <font color="blue"> [Creative
Commons Attribution-NonCommercial-ShareAlike 4.0 International License.](http://creativecommons.org/licenses/by-nc-sa/4.0/)</font>

# Introduction


## On Running Software

## 运行时软件
There’s something rather unique in Erlang in how it approaches failure compared to most other programming languages. There’s this common way of thinking where the language, programming environment, and methodology do everything possible to prevent errors. Something going wrong at run-time is something that needs to be prevented, and if it
cannot be prevented, then it’s out of scope for whatever solution people have been thinking about.

The program is written once, and after that, it’s off to production, whatever may happen
there. If there are errors, new versions will need to be shipped.

<font color="green">
相对于大多数其它编程语言，Erlang处理失败(failure)方法非常独特的。 常见处理错误的思维模式就是用尽一切方法来防止错误的发生，不允许在运行时出错，如果不能杜绝错误，系统就会超出设计者原本的设想（变得不可控）。<br>

一旦程序写完，并发布成产品出去，在什么都可能发生的生产环境下，如果出错了，那么必须要重新发布一个新版本修复这个问题。  </font>
<p></p>
Erlang, on the other hand, takes the approach that failures will happen no matter what,
whether they’re developer-, operator-, or hardware-related. It is rarely practical or even possible to get rid of all errors in a program or a system. <sup>1</sup> If you can deal with some errors rather than preventing them at all cost, then most undefined behaviours of a program can go in that "deal with it" approach.
<p></p>
<font color="green">
但Erlang能够处理错误，这些错误可以是运行时来自开发者设计本身的，操作符引起的或与硬件相关的任何失败，甚至可以处理来自于程序或系统的所有错误<sup>1</sup>。如果你用处理错误的方式来取代千方百计地阻止错误发生，那么所有的程序的不确定行为都会跑到这个错误处理的方法里。
</font>
<p></p>
This is where the "Let it Crash" <sup>2</sup> idea comes from: Because you can now deal with failure, and because the cost of weeding out all of the complex bugs from a system before it hits production is often prohibitive, programmers should only deal with the errors they know how to handle, and leave the rest for another process (a supervisor) or the virtual machine to deal with.<br>
Given that most bugs are transient <sup>3</sup>, simply restarting processes back to a state known to be stable when encountering an error can be a surprisingly good strategy.
<p></p>
<font color="green">
这就是"Let it Crash"<sup>2</sup> 的Erlang理念.这个idea来处自于：在系统运行至生产环境前，如果想把所有复杂的bugs都发现并除掉几乎是不可能，设计者可以只先处理他们知道原因的错误，让其它的都交给监控进程或另一个虚拟机(virtual machine)来处理。<br>
鉴于大多数bugs是短暂的<sup>3</sup>，简单的重启发生错误的进程让其重新工作到正常状态无疑是一个非常好的策略。
</font>
<p></p>
Erlang is a programming environment where the approach taken is equivalent to the human body’s immune system, where as most other languages only care about hygiene to make sure no germ enters the body. Both forms appear extremely important to me. Almost every environment offers varying degrees of hygiene. Nearly no other environment offers the immune system where errors at run time can be dealt with and seen as survivable.
<p></p>
<font color="green">
Erlang 这种处理方式非常像人类身体的免疫系统，其它语言只是关注环境中否足够干净得可以防止病菌进入身体。这两者的区别对我来说非常重要，因为几乎所有的环境都只提供不同程度的卫生，但是其它的系统都不能提供类似于免疫系统一样，可以在成长时容许病毒进入，并把病毒隔离或消灭掉。</font>
<p></p>
Because the system doesn’t collapse the first time something bad touches it, Erlang/OTP
also allows you to be a doctor. You can go in the system, pry it open right there in production, carefully observe everything inside as it runs, and even try to fix it interactively.
To continue with the analogy, Erlang allows you to perform extensive tests to diagnose the
problem and various degrees of surgery (even very invasive surgery), without the patients
needing to sit down or interrupt their daily activities.
<p></p>
<font color="green">
人体免疫系统在运行时可以处理错误被视为生存的希望。因为系统不会在第一次遇到坏情况时崩溃掉，Erlang/OTP 也可以让你来担当这样一位医生，让你进入系统，把行为纠正，仔细查看运行时的所有消息，甚至可以一步步交互式地去修复它。
Erlang 允许你做广泛的测试来诊断问题，在不中断病人的日常活动情况下执行不同程序的手术(甚至开刀手术)。</font>
<p></p>
This book intends to be a little guide about how to be the Erlang medic in a time of
war. It is first and foremost a collection of tips and tricks to help understand where failures come from, and a dictionary of different co de snipp ets and practices that help ed develop ers
debug pro duction systems that were built in Erlang.
<p></p>
<font color="green">
这本书意在：怎样在这个战争的时代作一位Erlang好医生。 首先你要学会收集的提示和技巧来了解失败是从哪里来的， 这是一本帮助开发人员调试建于Erlang的生产系统代码词典。
</font>
<p></p>

 **Who is this for?**
<p></p>
**这本书适合什么人？**
<p></p>

This book is not for beginners. There is a gap left between most tutorials, books, training sessions, and actually being able to operate, diagnose, and debug running systems once they’ve made it to production. There’s a fumbling phase implicit to a programmer’s learning of a new language and environment where they just have to figure how to get out of the guidelines and step into the real world, with the community that go es with it.
<p></p>
<font color="green">
这本书不适合于初学者。
大多数的书都没有讲如何在生产环境中操作，诊断，和调试运行时系统，但这个摸索阶段却隐含着一个程序员学习新的语言和环境时，如何起出指南和进入现实世界，与社区共同成长的过程。</font>
<p></p>
This book assumes that the reader is proficient in basic Erlang and the OTP framework.
Erlang/OTP features are explained as I see fit — usually when I consider them tricky —
and it is expected that a reader who feels confused by usual Erlang/OTP material will have an idea of where to lo ok for explanations if necessary 4.
What is not necessarily assumed is that the reader knows how to debug Erlang software,
dive into an existing code base, diagnose issues, or has an idea of the best practices about deploying Erlang in a production environment 5.
<p></p>
<font color="green">
阅读此书需要读者精通(proficient)基本的Erlang和OTP框架，我只会在认为OTP很棘手的时候解释下。希望感到困惑的读者能仔细阅读Erlang/OTP相关的资料。
但这并不需要读者会调试 Erlang 软件，深入现有的代码库，诊断问题，或掌握在生产环境中部署Erlang的最佳实践。</font>
<p></p>
**How To Read This Book**
<p></p>
**如何去读这本书？**
<p></p>
This book is divided in two parts.
Part I focuses on how to write applications. It includes how to dive into a code base
(Chapter 1), general tips on writing open source Erlang software (Chapter 2), and how to
plan for overload in your system design (Chapter 3).<br>
Part II focuses on being an Erlang medic and concerns existing, living systems. It
contains instructions on how to connect to a running node (Chapter 4), and the basic
runtime metrics available (Chapter 5). It also explains how to perform a system autopsy
using a crash dump (Chapter 6), how to identify and fix memory leaks (Chapter 7), and
how to find runaway CPU usage (Chapter 8). The final chapter contains instructions on
how to trace Erlang function calls in production using recon <sup>6</sup> to understand issues before they bring the system down (Chapter 9).<br>
Each chapter is followed up by a few optional exercises in the form of questions or
hands-on things to try if you feel like making sure you understood everything, or if you
want to push things further.
<p></p>
<font color="green">
此书分两部分：<br>

Part I 集中讲怎么去写一个应用(applications).<br>

包括:<br>
章节1：怎样了解现有的代码库;<br>
章节2：常用写开源Erlang软件的注意项;<br>
章节3：如何在为系统过载做计划设计.<br>

 Part II 集中于做为一个Erlang医生怎么去关注现有的生命系统.<br><br>

包括:<br>
章节4：如何连接一个运行时的节点;<br>
章节5：基本的运行时状态指标信息;<br>
章节6：如何通过crash dump文件来解剖系统;<br>
章节7：如何识别和修复内存泄露;<br>
章节8：如何找到CPU失控的原因;<br>
章节9：怎样去在生产环境中使用recon<sup>6</sup>来追踪(trace)Erlang函数，从而在问题把系统搞挂之前就定位到它。<br>

每个章节都有一些练习，可用于动手尝试确保你明白了一切，这会让你理解更深刻。
</font>

<p></p>
[1]. life-critical systems are usually excluded from this category<br>
[2]. Erlang people now seem to favour "let it fail", given it makes people far less nervous.<br>
[3]. 131 out of 132 bugs are transient bugs (they’re non-deterministic and go away when you look at them,
and trying again may solve the problem entirely), according to Jim Gray in Why Do Computers Stop and What Can Be Done About It?<br>
[4]. I do r ecommend visiting Learn You Some Erlang or the regular Erlang Documentation if a free resource
is required<br>
[5]. Running Erlang in a screen or tmux session is not a deployment strategy.<br>
[6]. [http://ferd.github.io/recon/](http://ferd.github.io/recon/) — a library used to make the text lighter, and with generally productionsafe functions.
<p><p>
<font color="green">
[注1]:生死攸关的系统不在这个讨论范畴。<br>
[注2]:更确切地说是"let it fail"，但这会让人更加紧张。<br>
[注3]:131/132的bugs都是短暂的bugs，他们出现得不确定，当你去找他们时，他们又消失了，再次尝试或许可以解决这个问题，根据Jim Gray的 Why Do Computers Stop and What Can Be Done About It?<br>
[注4]:非常推荐你先看看Learn You Some Erlang 或 Erlang的官方文档。<br>
[注5]:在屏幕或tmux session运行Erlang并不是一个部署策略。<br>
[注6]:[http://ferd.github.io/recon/](http://ferd.github.io/recon/) 一个轻量级的用于tracer 代码库。
</font>

