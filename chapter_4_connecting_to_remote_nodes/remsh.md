# Remsh
There’s a mechanism entirely similar to the one available through the JCL mode, although invoked in a different manner. The entire JCL mode sequence can by bypassed by starting the shell as follows for long names:
<p></p> <font color="green">
&emsp;还有一种类似于JCL模式的机制，只是他们启动稍有不同。整个JCL模式序列可以使用长命名启动的shell.<br>
</font> <p></p>

---------------------------------------------------------------------------------------<br>
`erl -name local@domain.name -remsh remote@domain.name`<br>
---------------------------------------------------------------------------------------<br>
And as follows for short names:<br>
<p></p> <font color="green">

&emsp;也可以使用短命名：<br>
</font> <p></p>
---------------------------------------------------------------------------------------<br>
1 `erl -sname local@domain -remsh remote@domain`<br>
---------------------------------------------------------------------------------------<br>
<p></p>
All other Erlang arguments (such as -hidden and -setcookie $COOKIE) are also valid.<br>
The underlying mechanisms are the same as when using JCL mode, but the initial shell is started remotely instead of locally (JCL is still local). ˆG remains the safest way to exit the remote shell.
<p></p> <font color="green">
&emsp;所有的其它Erlang参数(比如：-hidden -setcookie $COOKIE)都是有效的。<br>
&emsp;底层机制和使用JCL模式是一样的，但启动的节点是远程的，崦JCL是本地的(使用remsh启动的节点如果输入q就会真的把远程节点都退出); Ctrl+G仍然是退出远程shell的最安全方式。
</font> <p></p>

