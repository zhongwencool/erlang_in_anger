# Code
# 代码所占内存
The code on an Erlang node is loaded in memory in its own area, and sits there until it is
garbage collected. Only two copies of a module can coexist at one time, so looking for very
large modules should be easy-ish.<br>
&emsp;If none of them stand out, look for code compiled with HiPE <sup>5</sup>. HiPE code, unlike
regular BEAM code, is native code and cannot be garbage collected from the VM when
new versions are loaded. Memory can accumulate, usually very slowly, if many or large
modules are native-compiled and loaded at run time.<br>
&emsp;Alternatively, you may look for weird modules you didn’t load yourself on the node and panic if someone got access to your system!

<p></p> <font color="green">
&emsp;Erlang节点上的代码会被加载到专门存代码的内存中，并一直储存在那里直到垃圾回收。一个模块在同一时间是能有2个版本(copies)。所以打出占非常多内存的模块也应该很简单。<br>
&emsp;如果他们都没有表现出非常大的异常，可以查看HiPE<sup>5</sup>编译过的代码，HiPE代码并不同于正常的BEAM代码，它是原生(native)代码，当一个新版本被加载时它不会被VM垃圾回收掉。内存积累得非常慢，但如果大量或单个非常大的模块都是native-compiled并在运行时加载的话，就会导致问题。<br>
&emsp;或者，你可以找一下那些不是你自己在节点上加载，但被别人进入你系统后加载的奇怪模块！
</font> <p></p>



[5] http://www.erlang.org/doc/man/HiPE_app.html<br>


<p></p> <font color="green">
[5] http://www.erlang.org/doc/man/HiPE_app.html<br>
</font> <p></p>
