# ETS
# ETS表占用内存
ETS tables are never garbage collected, and will maintain their memory usage as long as
records will be left undeleted in a table. Only removing records manually (or deleting the
table) will reclaim memory.<br>
In the rare cases you’re actually leaking ETS data, call the undocumented **ets:i()**
function in the shell. It will print out information regarding number of entries (size) and
how much memory they take (mem). Figure out if anything is bad.<br>
It’s entirely possible all the data there is legit, and you’re facing the difficult problem
of needing to shard your data set and distribute it over many nodes. This is out of scope
for this book, so best of luck to you. You can look into compression of your tables if you
need to buy time, however. <sup>6</sup>

<p></p> <font color="green">
&emsp;ETS表不会被垃圾回收，会一直保存，直到存在里面的records从表中被删除掉。只有手动移除records(或删除表)会回收内存。<br>
&emsp;在极少数情况下，你真的会有ETS数据泄露，在shell中使用一个未写入文档的函数：**ets:i()**。它会打印出记录的数量(size)和他们使用了多少内存(mem)信息。找找是不是有什么奇怪的东西出了状况。<br>
&emsp;还完全有可能所有数据都是合理的。那你就面临着难题：考虑切分你的数据集并分发它到多个节点上。这已超过了本书所讲的范畴了。那么祝你走运。如果你要争取时间(尽快修复它的时间)的话，你可以考虑下压缩表<sup>6</sup>
</font> <p></p>

[6]See the compressed option for ets:new/2<br>

<p></p> <font color="green">
[注6]：压缩参数可以使用：ets:new/2来指定<br>
</font> <p></p>

