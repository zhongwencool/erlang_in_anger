# Job Control Mode
# 作业控制模式
The Job Control Mode (JCL mode) is the menu you get when you press ˆG in the Erlang shell. From that menu, there is an option allowing you to connect to a remote shell:
<p></p> <font color="green">
作业控制模式(JCLmode)是当你在Erlang shell中按下Ctrl+G时出现在菜单。里面有一个选项允许人连接到一个远程节点上：
</font> <p></p>
--------------------------------------------------------------------------------------<br>
`(somenode@ferdmbp.local)1>`<br>
`User switch command`<br>
`--> h`<br>
`c [nn] - connect to job`<br>
`i [nn] - interrupt job`<br>
`k [nn] - kill job`<br>
`j - list all jobs`<br>
`s [shell] - start local shell`<br>
`r [node [shell]] - start remote shell`<br>
`q - quit erlang`<br>
`? | h - this message`<br>
`--> r ’server@ferdmbp.local’`<br>
`--> c`<br>
`Eshell Vx.x.x (abort with ^G)`<br>
`(server@ferdmbp.local)1>`<br>
--------------------------------------------------------------------------------------<br>
<p></p>

When that happens, the local shell runs all the line editing and job management locally, but the evaluation is actually done remotely. All output coming from said remote evaluation will be forwarded to the local shell.<br>
To quit the shell, go back in the JCL mode with ˆG. This job management is, as I said, done locally, and it is thus safe to quit with ˆG q:
<p></p> <font color="green">
当完成了上面操作后，那些你看上去在本地shell完成输入行或作业管理(job management)，实际上是远程节点上完成的。所以远程节点的运行结果也在本地节点上显示。<br>
为了限制shell(远程节点的shell)你可以使用Ctrl+G回来JCL模式，然后输入q退出本地节点(不会影响远程节点).
--------------------------------------------------------------------------------------<br>
`(server@ferdmbp.local)1>`<br>
`User switch command`<br>
`--> q`<br>
--------------------------------------------------------------------------------------<br>
</font> <p></p>

You may choose to start the initial shell in hidden mode (with the argument -hidden) to avoid connecting to an entire cluster automatically.
<p></p> <font color="green">
你可以使用hidden模式(使用参数：-hidden)开启一个新的shell，这样就可以防止这个shell自动连接到整个集群节点上了。
</font> <p></p>
