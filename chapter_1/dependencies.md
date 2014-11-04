# Dependencies
All applications have dependencies<sup>5</sup>, and these dependencies will have their own dependencies. OTP applications usually share no state between them, so it’s possible to know what bits of code depend on what other bits of code by looking at the app file only, assuming the developer wrote them in a mostly correct manner.<br>
Figure 1.1 shows a diagram that can be generated from looking at app files to help understand the structure of OTP applications.
<p></p>
<font color="green">
&emsp;所有的applications都有依赖性(dependencies),这些被依赖的application也还有自己的依赖application<sup>5</sup>。OTP applications之间通常是不共享状态的，所以你可以只通过app文件就知道这个app大概如何使用，当然是假定这个application是开发者是用正确的方式写出来的前提下。<br>
Figure 1.1 能过app文件生成的图表来理解OTP applications结构.
</font>
<p></p>
Using such a hierarchy and looking at each application’s short description might be helpful to draw a rough, general map of where everything is located. To generate a similar diagram, find recon’s script directory and call escript script/app_deps.erl<sup>6</sup>. Similar
<p></p>
<font color="green">
&emsp;使用这种层次结构和查看每个applications的简明描述，就可以画一个粗略，但包含所有关键元素所在的地图来.你也可以使用recon脚本：调用escript script/app_deps.erl 来生成类似的图表<sup>6</sup>。
</font>
<p></p>

![](http://erlang-in-anger.qiniudn.com/chapter1_1.png)

Figure 1.1: Dependency graph of riak_cs, Basho’s open source cloud library. The graph ignores dependencies on common applications like kernel and stdlib. Ovals are applications,
rectangles are library applications.
<p></p>
<font color="green">
Figure1.1:riak_cs的依赖图示，Boaho开源云代码库，本图忽略了像kernel和stdlib类常用的依赖applications，
长方形表示library applications.
</font>
<p></p>
hierarchies can be found using the observer <sup>7</sup> application, but for individual supervision trees. Put together, you may get an easy way to find out what does what in the code base.
<p></p>
<font color="green">
&emsp;层次关系还可以使用observer <sup>7</sup> application来查看，但只是针对个别supervisior树。把所有的汇总，你就可以轻松地深入代码。
</font>
<p></p>
[5]At the very least on the kernel and stdlib applications<br>
[6]This script depends on graphviz<br>
[7]http://www.erlang.org/doc/apps/observer/observer_ug.html
<p></p>
<font color="green">
[注5]：至少会依赖kernel和stdlib. <br>
[注6]:此脚本依赖于画图工具(graphviz). <br>
[注7]：http://www.erlang.org/doc/apps/observer/observer_ug.html
</font>
