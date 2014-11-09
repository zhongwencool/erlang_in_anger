# Detecting Leaks
Detecting leaks for reference-counted binaries is easy enough: take a measure of all of
each process’ list of binary references (using the binary attribute), force a global garbage collection, take another snapshot, and calculate the difference.
<br>&emsp;This can be done directly with recon:bin_leak(Max) and looking at the node’s total memory before and after the call:<br>
<p></p> <font color="green">
&emsp;检测引用记数的binaries泄露非常容易：对所有的进程列表的binary references(使用binary 属性)都采取措施，强制一个全局的垃圾回收，把结果与回收前的对比一下，计算下差异。<br>
&emsp;你可以直接使用recon:bin_leak(Max)来完成它，并在调用前后比较节点的总内存变化：
</font> <p></p>

--------------------------------------------------------<br>
`1> recon:bin_leak(5).`<br>
`[{<0.4612.0>,-5580,`<br>
`[{current_function,{gen_fsm,loop,7}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.17479.0>,-3724,`<br>
`[{current_function,{gen_fsm,loop,7}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.31798.0>,-3648,`<br>
`[{current_function,{gen_fsm,loop,7}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.31797.0>,-3266,`<br>
`[{current_function,{gen,do_call,4}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.22711.1>,-2532,`<br>
`[{current_function,{gen_fsm,loop,7}},`<br>
`{initial_call,{proc_lib,init_p,5}}]}]`<br>
--------------------------------------------------------<br>

<br>&emsp;This will show how many individual binaries were held and then freed by each process
as a delta. The value -5580 means there were 5580 fewer refc binaries after the call than
before.
<br>&emsp;It is normal to have a given amount of them stored at any point in time, and not all numbers are a sign that something is bad. If you see the memory used by the VM go down drastically after running this call, you may have had a lot of idling refc binaries.
<br>&emsp;Similarly, if you instead see some processes hold impressively large numbers of them <sup>9</sup>,
that might be a good sign you have a problem.
<br>&emsp;You can further validate the top consumers in total binary memory by using the special **binary_memory** attribute supported in **recon**:<br>
<p></p> <font color="green">
&emsp;这样就可以看出有多少每个进程有多少私有的binaries和释放掉了多少。-5580表示有调用函数后此进程少了5580的refc binaries<br>
&emsp;任何时候都存有大量的refc binaries是正常的，所以并不是上面所有的数据都能说明出问题了。如果你看到调用过这个函数后内存下降得非常厉害。你就可能会有很多懒惰的refc binaries存在。<br>
&emsp;同理，如果调用后，你还是看到很多进程拥有大量的refc binaries<sup>9</sup>。那么也能说明这里有问题。<br>
&emsp;接下来，你可以使用**recon**里面的**binary_memory**属性来验证消费者的总binary内存前几名的使用情况。
</font> <p></p>

--------------------------------------------------------<br>
`1> recon:proc_count(binary_memory, 3).`<br>
`[{<0.169.0>,77301349,`<br>
`[app_sup,`<br>
`{current_function,{gen_server,loop,6}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.21928.1>,9733935,`<br>
`[{current_function,{erlang,hibernate,3}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.12386.1172>,7208179,`<br>
`[{current_function,{erlang,hibernate,3}},`<br>
`{initial_call,{proc_lib,init_p,5}}]}]`<br>
--------------------------------------------------------<br>
<br>&emsp;This will return the N top processes sorted by the amount of memory the refc binaries reference to hold, and can help point to specific processes that hold a few large binaries, instead
of their raw amount. You may want to try running this function before recon:bin_leak/1,
given the latter garbage collects the entire node first.
<p></p> <font color="green">
&emsp;这会返回refc binaries reference总量前N个的进程信息，这可以帮助找到那些拥有大量binaries的进程。你可以在使用recon:bin_leak/1前先运行下这个函数，先看看整个节点的垃圾回收状态。
</font> <p></p>

[9] We’ve seen some processes hold hundreds of thousands of them during leak investigations at Heroku!
<p></p> <font color="green">
[注9] 在Heroku的泄露事件调查中，我们可以看到一些进程有成千上万refc binaries。
</font> <p></p>


