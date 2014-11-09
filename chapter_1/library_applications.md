# Library Applications

Library applications will usually have modules named appname _something, and one module named appname . This will usually be the interface module that’s central to the library and contains a quick way into most of the functionality provided.<br>
By looking at the source of the module, you can figure out how it works with little effort: If the module adheres to any given behaviour (gen_server, gen_fsm, etc.), you’re most likely expected to start a process under one of your own supervisors and call it that way.<br>
If no behaviour is included, then you probably have a functional, stateless library on your hands. For this case, the module’s exported functions should give you a quick way to understand its purpose.
<p></p>
<font color="green">
&emsp;Library application通常会有一些叫appname_XXX和一个叫appname的模块名组成，他们通常是连接libary核心的接口模块，或提供搞定大部分功能的便捷函数。<br>
&emsp;通过看这些模块的源代码，你会明白：它是如何与behaviour(gen_server,gen_fsm等)相互协调工作的。你遇到的情况最有可能是：在自己的监控进程(supervisors)创建一个普通的工作进程。<br>
&emsp;如果没有包括behaviour, 这些代码很可能是一个没有状态信息(stateless)的函数工具库，对于这种情况，这个模块导出的函数是为了让你快速地理解它们。
</font>
