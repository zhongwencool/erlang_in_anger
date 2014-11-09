# Chapter 2 Building Open Source Erlang Software
# 创建一个开源的Erlang软件(software)

Most Erlang books tend to explain how to build Erlang/OTP applications, but few of them go very much in depth about how to integrate with the Erlang community doing Open Source work.<br>
Some of them even avoid the topic on purpose. This chapter dedicates itself to doing a quick tour of the state of affairs in Erlang.<br>
OTP applications are the vast majority of the open source code people will encounter.<br>
In fact, many people who would need to build an OTP release would do so as one umbrella OTP application.
<p></p>
<font color="green">
&emsp;大多数介绍Erlang的书都倾向于介绍怎样去创建Erlang/OTP applications，但鲜有更深入去介绍如何与Erlang社区集成做一些开源的工作(Open Source Work)。<br>
&emsp;他们中甚至有人故意回避这个主题。本章节致力示范下如何快速完成这些工作。<br>
&emsp;OTP applications是人们在绝大部分的开源代码中都会遇到的。
事实上，很多需要创建OTP release的人会只是做一个伞形结构的OTP application。
</font>
<p></p>
If what you’re writing is a stand-alone piece of code that could be used by someone building a product, it’s likely an OTP application. If what you’re building is a product that stands on its own and should be deployed by users as-is (or with a little configuration), what you should be building is an OTP release. <sup>1</sup><br>
The main build tools supported are rebar and erlang.mk. The former is a portable Erlang script that will be used to wrap around a lot of standard functionality and add its own, while the latter is a very fancy makefile that does a bit less, but tends to be faster when it comes to compiling.<br>
 In this chapter, I’ll mostly focus on using rebar to build things, given it’s the ad-hoc standard, is well-established, and erlang.mk applications tend to also be supported by rebar as dependencies.
<p></p>
<font color="green">
&emsp;如果你正在写一个独立代码库(供给别人创建产品,产品组成的某部分)，最好是写成OTP application。 如果你创建的代码(product)全部基于自己本身(stands on its own)并提供给用户部署的，你应该创建的是一个OTP release。<br>
最主要的构建工具是rebar 和erlang.mk. rebar是一个附带许多标准功能的便携Erlang脚本,erlang.mk则是一个很小但却可以是非常快编译文件的神奇makefile 文件。<br>
在本章节中，我大部分会集中在使用rebar来创建符合标准的文件，rebar也支持erlank.mk 的application。
</font>
<p></p>
[1]The details of how to build an OTP application or release is left up to the Erlang introduction book you have at hand.
<p></p>
<font color="green">
[注1]：关于如何创建一个OTP application 或OTP release，你可以查阅其它介绍Erlang的书籍。
</font>
