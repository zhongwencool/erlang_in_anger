# Fixing Memory Fragmentation with a Different Allocation Strategy
Tweaking your VM’s options for memory allocation may help.
<br>&emsp;You will likely need to have a good understanding of what your type of memory load and usage is, and be ready to do a lot of in-depth testing. The recon_alloc module contains a few helper functions to provide guidance, and the module’s documentation <sup>22</sup> should be read at this point.
<br>&emsp;You will need to figure out what the average data size is, the frequency of allocation and deallocation, whether the data fits in mbcs or sbcs, and you will then need to try playing with a bunch of the options mentioned in recon_alloc, try the different strategies, deploy them, and see if things improve or if they impact times negatively.
<br>&emsp;This is a very long process for which there is no shortcut, and if issues happen only every few months per node, you may be in for the long haul.

<p></p> <font color="green">
&emsp;调整你的虚拟机的内存分配的选择可能会有所帮助。<br>
&emsp;你可能需要很好地理解你的内存负载类型和使用情况，然后准备好做大量深入的测试。recon_alloc模块有一些帮助函数来指供指导，模块的文档<sup>22</sup>。<br>
&emsp;你要找出数据的平均大小是多少，分配与释放的频率，数据使用的是mbcs还是sbcs，然后你还将要尝试玩一下recon_alloc里面提及到的一堆选项，尝试不同的策略，布署他们，然后看看是不是有改善或变得更加糟糕。<br>
&emsp;这是一个非常漫长的过程,没有捷径可走。如果问题的每个节点只会每几个月发生一次，那么你可能要长期艰苦奋战。
</font> <p></p>


[22] http://ferd.github.io/recon/recon_alloc.html

<p></p> <font color="green">
[注22]： http://ferd.github.io/recon/recon_alloc.html
</font> <p></p>

