# Part I：Writing Applications

# Chapter 1 How to Dive into a Code Base

## 如何深入代码库
<p></p>
"Read the source" is one of the most annoying things to be told, but dealing with Erlang programmers, you’ll have to do it often. Either the documentation for a library will be incomplete, outdated, or just not there.<br>
In other cases, Erlang programmers are a bit similar to Lispers in that they will tend to write libraries that will solve their problems and not really test or try them in other circumstances, leaving it to you to extend or fix issues that arise in new contexts.<br>
It’s thus pretty much guaranteed you’ll have to go dive in some code base you know nothing about, either because you inherited it at work, or because you need to fix it or understand it to be able to move forward with your own system.
This is in fact true of most languages whenever the project you work on is not one you designed yourself.
<p></p>
<font color="green">
&emsp;“阅读源码” 是最讨人厌的忠告之一，但是对于编写Erlang来说，你不得不经常阅读源码，要么代码文档不全，过时了，或完全不存在。<br>
&emsp;另一方面来说，Erlang编程人员处境有点类似于Lispers,他们倾向于自己写代码库来解决自己问题，根本不尝试着让这些代码适用于其它情景，如果想在新场景应用它们，只得你自己去扩展或修复其中的问题。<br>
&emsp;因此，你不得不深入源码才能确保代码的可靠性，不论你是接管上一位的工作，还是需要修复一个问题，或理解它并把他移植到你自己的系统中。
工作的项目不是自己设计时，“阅读源码”无疑是最有效的方法。
</font>
<p></p>
There are three main types of Erlang code bases you’ll encounter in the wild: raw Erlang code bases, OTP applications, and OTP releases. In this chapter, we’ll look at each of these and try to provide helpful tips on navigating them.
<p></p>
<font color="green">
&emsp;一般你会遇到三种类型的Erlang代码：未加工过的(Raw)Erlang代码库，OTP Applications 和OTP releases. 接下来，我们逐一地分析它们，并提供一些有用的建议以便更好地驾驭它们。
</font>
<p></p>



















