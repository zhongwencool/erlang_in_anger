# Named Pipes
# 命名管道(Named Pipes)
A little known way to connect with an Erlang node that requires no explicit distribution is through named pipes. This can be done by starting Erlang with run_erl, which wraps Erlang in a named pipe 5:
<p></p> <font color="green">
&emsp;还有一种鲜为人知连接Erlang节点的方法：命名管道(named pipes)，它不需要明确指定节点名.这可以通过使用run_erl 启动Erlang来完成<sup>5</sup>。
</font> <p></p>
-------------------------------------------------------------------------------------<br>
`$ run_erl  /tmp/erl_pipe  /tmp/log_dir  "erl"`<br>
-------------------------------------------------------------------------------------<br>
The first argument is the name of the file that will act as the named pipe. The second one is where logs will be saved <sup>6</sup>.<br>
To connect to the node, you use the to_erl program:
<p></p> <font color="green">
&emsp;第一个参数是指定作为命名管道的文件，第三个就是指明日志要放在目录<sup>6</sup>。<br>
&emsp;你可以使用 to_erl程序连接到节点上：
</font> <p></p>
-------------------------------------------------------------------------------------<br>
`$ to_erl  /tmp/erl_pipe`<br>
`Attaching to /tmp/erl_pipe (^D to exit)`<br>
`1>`<br>
-------------------------------------------------------------------------------------<br>
 <p></p>
And the shell is connected. Closing stdio (with ˆD) will disconnect from the shell while leaving it running.
<p></p> <font color="green">
&emsp;连接上后，你可以使用ctrl+D来断开远程节点(只是断开，不会终结远程节点).
</font> <p></p>
[5] "erl" is the command being run. Additional arguments can be added after it. For example
"erl +K true" will turn kernel polling on.<br>
[6] Using this method ends up calling fsync for each piece of output, which may give quite a performance hit if a lot of IO is taking place over standard output.<br>
<p></p> <font color="green">
[注5]：“erl”就是要运行的命令，你也可以在后面加上其它启动选项如"erl +K ture"会把打开内核kernel轮询。<br>
[注6] ：这个方法最终fsync会同步每一块输入，如果你把大量的数据都输入到终端，性能会下降得很厉害。
</font> <p></p>
