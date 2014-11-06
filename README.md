<< Stuff Goes Bad: Erlang in Anger >> 由 Fred Hébert(写Learn Your Some Erlang的帅小伙) 和 Heroku 联合推出，遵循 <font color="blue"> [Creative
Commons Attribution-NonCommercial-ShareAlike 4.0 International License.](http://creativecommons.org/licenses/by-nc-sa/4.0/)</font>

# Introduction


## On Running Software

## 运行时软件
There’s something rather unique in Erlang in how it approaches failure compared to most other programming languages. There’s this common way of thinking where the language, programming environment, and methodology do everything possible to prevent errors. Something going wrong at run-time is something that needs to be prevented, and if it cannot be prevented, then it’s out of scope for whatever solution people have been thinking about.<br>
The program is written once, and after that, it’s off to production, whatever may happen
there. If there are errors, new versions will need to be shipped.

<font color="green">
&emsp;相对于大多数其它编程语言，Erlang处理失败(failure)方法非常独特的。 常见处理错误的思维模式是费尽心机防止发生错误，不允许在运行时出错，如果不能杜绝错误，它的行为就会超出设计者原本的设想（变得不可控）。<br>

&emsp;一旦程序写完，并发布为产品放出去，如果程序在这个变化莫测的生产环境中出了问题，，那么必须为修复它重新发布新版本。  </font>
<p></p>
Erlang, on the other hand, takes the approach that failures will happen no matter what,
whether they’re developer-, operator-, or hardware-related. It is rarely practical or even possible to get rid of all errors in a program or a system. <sup>1</sup> If you can deal with some errors rather than preventing them at all cost, then most undefined behaviours of a program can go in that "deal with it" approach.
<p></p>
<font color="green">
&emsp;但Erlang有办法对付这种错误，不管它是开发者设计不周引起的，还是操作符引起的或与硬件相关的因素，它甚至有可能驾驭来自于程序或系统的所有错误<sup>1</sup>。如果你能处理错误，而不是千方百计去杜绝错误，那么程序所有的不确定行为都会跑到指定的错误处理方法中。
</font>
<p></p>
This is where the "Let it Crash" <sup>2</sup> idea comes from: Because you can now deal with failure, and because the cost of weeding out all of the complex bugs from a system before it hits production is often prohibitive, programmers should only deal with the errors they know how to handle, and leave the rest for another process (a supervisor) or the virtual machine to deal with.<br>
Given that most bugs are transient <sup>3</sup>, simply restarting processes back to a state known to be stable when encountering an error can be a surprisingly good strategy.
<p></p>
<font color="green">
&emsp;这就是"Let it Crash"<sup>2</sup> 的Erlang理念。这个idea起源于：如果想在系统部署到生产环境前，找出所有复杂的bugs，并把它们斩尽杀绝，这几乎是不可能的。设计者应当先处理他们知道原因的错误，让其它不可遇料的错都交给另一个进程(supervisor进程)或虚拟机(virtual machine)来处理。<br>
鉴于大多数bugs转瞬即逝<sup>3</sup>，简单地重启出现错误的进程，让其重新工作到正常状态，无疑是一个非常好的策略。
</font>
<p></p>
Erlang is a programming environment where the approach taken is equivalent to the human body’s immune system, where as most other languages only care about hygiene to make sure no germ enters the body. Both forms appear extremely important to me. Almost every environment offers varying degrees of hygiene. Nearly no other environment offers the immune system where errors at run time can be dealt with and seen as survivable.
<p></p>
<font color="green">
&emsp;Erlang 这种处理方式与类身体的免疫系统非常像，但其它语言只是关注环境是否可以防止病菌进入身体。这两者的区别对我来说非常重要，因为几乎所有的环境都只提供不同程度的卫生，几乎没有其它系统能提供类似于免疫系统一样机制，可以在运行时容许不明病毒进入，并把病毒隔离或消灭掉。</font>
<p></p>
Because the system doesn’t collapse the first time something bad touches it, Erlang/OTP
also allows you to be a doctor. You can go in the system, pry it open right there in production, carefully observe everything inside as it runs, and even try to fix it interactively.
To continue with the analogy, Erlang allows you to perform extensive tests to diagnose the
problem and various degrees of surgery (even very invasive surgery), without the patients
needing to sit down or interrupt their daily activities.
<p></p>
<font color="green">
&emsp;人体免疫系统运行时可以处理病毒被视为生存的希望。这种机制使得系统不会在第一次遇到错误时崩溃掉，Erlang/OTP也可以让你成为一位这样的医生，你可以深入系统，在运行时纠正错误行为，仔细查看运行时的所有信息，甚至可以一步步交互式地去修复它。
Erlang 允许你做各种各样的测试来诊断问题，在不打扰病人的日常活动情况下执行不同程度的手术(甚至开刀手术)。</font>
<p></p>
This book intends to be a little guide about how to be the Erlang medic in a time of
war. It is first and foremost a collection of tips and tricks to help understand where failures come from, and a dictionary of different code snipp ets and practices that helped developers
debug production systems that were built in Erlang.
<p></p>
<font color="green">
&emsp;这本书意在：怎样在这个战争的时代做个称职的Erlang医生。 首先，也是最重要的：你要学会收集信息的相关技巧，以便理解失败是从哪里来的， 这是一本帮助开发人员调试用Erlang构建的生产系统代码词典。
</font>
<p></p>

 **Who is this for?**
<p></p>
**这本书适合什么人？**
<p></p>

This book is not for beginners. There is a gap left between most tutorials, books, training sessions, and actually being able to operate, diagnose, and debug running systems once they’ve made it to production. There’s a fumbling phase implicit to a programmer’s learning of a new language and environment where they just have to figure how to get out of the guidelines and step into the real world, with the community that go es with it.
<p></p>
<font color="green">
&emsp;这本书不适合于初学者。
大多数的Erlang书都没有讲如何在生产环境中操作，诊断，和调试运行时系统，但这个摸索阶段却隐含着一个程序员学习新的语言和环境时，摆脱纸上谈兵并进入现实世界，与社区共同成长的过程。</font>
<p></p>
This book assumes that the reader is proficient in basic Erlang and the OTP framework.
Erlang/OTP features are explained as I see fit — usually when I consider them tricky —
and it is expected that a reader who feels confused by usual Erlang/OTP material will have an idea of where to lo ok for explanations if necessary 4.
What is not necessarily assumed is that the reader knows how to debug Erlang software,
dive into an existing code base, diagnose issues, or has an idea of the best practices about deploying Erlang in a production environment 5.
<p></p>
<font color="green">
&emsp;阅读此书需要读者精通(proficient)基本的Erlang知识和OTP框架，我只会在认为OTP很棘手时解释。希望感到困惑的读者能仔细阅读Erlang/OTP相关的资料。但这并不需要读者掌握调试Erlang 软件，深入现有的代码库，诊断问题，或掌握在生产环境中部署Erlang的最佳实践。</font>
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
&emsp;此书分两部分：<br>

Part I 集中讲怎么去写一个Applications.<br>

包括:<br>
章节1：怎样了解现有的代码库;<br>
章节2：常用写开源Erlang软件的注意项;<br>
章节3：如何在为系统过载规划设计.<br>

 Part II 集中于描述一个称职的Erlang医生怎么去关注现有的生命系统中的各项指标.<br><br>

包括:<br>
章节4：如何连接一个运行中的节点;<br>
章节5：基本的运行时状态指标信息;<br>
章节6：如何通过crash dump文件来解剖系统;<br>
章节7：如何识别和修复内存泄露;<br>
章节8：如何找到CPU失控的原因;<br>
章节9：怎样去在生产环境中使用recon<sup>6</sup>来trace Erlang函数，从而在系统崩溃前就定位到问题。<br>

每个章节都有练习，其中有一些可用于动手尝试，让你更上一层楼。
</font>

<p></p>
[1]. life-critical systems are usually excluded from this category<br>
[2]. Erlang people now seem to favour "let it fail", given it makes people far less nervous.<br>
[3]. 131 out of 132 bugs are transient bugs (they’re non-deterministic and go away when you look at them,
and trying again may solve the problem entirely), according to Jim Gray in Why Do Computers Stop and What Can Be Done About It?<br>
[4]. I do recommend visiting Learn You Some Erlang or the regular Erlang Documentation if a free resource
is required<br>
[5]. Running Erlang in a screen or tmux session is not a deployment strategy.<br>
[6]. [http://ferd.github.io/recon/](http://ferd.github.io/recon/) — a library used to make the text lighter, and with generally productionsafe functions.
<p><p>
<font color="green">
[注1]:生死攸关的系统不在这个讨论范畴。<br>
[注2]:更确切地说是"let it fail"，但这会让人更加紧张。<br>
[注3]:131/132的bugs都是短暂的bugs(Bugs出现得不确定，当你去找他们时，他们又消失了，再次尝试或许可以解决这个问题)根据Jim Gray的 Why Do Computers Stop and What Can Be Done About It?<br>
[注4]:非常推荐你先看看Learn You Some Erlang 或 Erlang的官方文档。<br>
[注5]:在屏幕中或tmux session运行Erlang并不能算得上是一个部署策略。<br>
[注6]:[http://ferd.github.io/recon/](http://ferd.github.io/recon/) 一个轻量级的用于tracer 代码库。
</font>

