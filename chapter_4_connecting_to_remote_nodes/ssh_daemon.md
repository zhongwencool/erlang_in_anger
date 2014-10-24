# SSH Daemon
Erlang/OTP comes shipped with an SSH implementation that can both act as a server and a client. Part of it is a demo application providing a remote shell working in Erlang.<br>
To get this to work, you usually need to have your keys to have access to SSH stuff remotely in place already, but for quick test purposes, you can get things working by doing:<br>
<p></p> <font color="green">
Erlang/OTP附带了一个SSH实现，它即可以做服务器，也可以做客户端。它的一部分可以看作是提供使用Erlang工作的远程shell演示application。<br>
为了让它工作起来，你通常需要把你的SSH keys在远程服务器上部署好。但为了快速体验到这个功能，你也可以像下面这样子做：
</font> <p></p>

-----------------------------------------------------------------------------------<br>
`$ mkdir /tmp/ssh`<br>
`$ ssh-keygen -t rsa -f /tmp/ssh/ssh_host_rsa_key`<br>
`$ ssh-keygen -t rsa1 -f /tmp/ssh/ssh_host_key`<br>
`$ ssh-keygen -t dsa -f /tmp/ssh/ssh_host_dsa_key`<br>
`$ erl`<br>
`1> application:ensure_all_started(ssh).`<br>
`{ok,[crypto,asn1,public_key,ssh]}`<br>
`2> ssh:daemon(8989, [{system_dir, "/tmp/ssh"},`<br>
`2> {user_dir, "/home/ferd/.ssh"}]).`<br>
`{ok,<0.52.0>}`<br>
-----------------------------------------------------------------------------------<br>
<p></p>
I’ve only set a few options here, namely system_dir, which is where the host files are, and user_dir, which contains SSH configuration files. There are plenty of other options available to allow for specific passwords, customize handling of public keys, and so on <sup>3</sup>.
<p></p> <font color="green">

在上面我只设置了几个选项，指定存放host文件的系统目录(system_dir),指定包含SSH配置文件的用户文件.其实还有大量可用其它选项：允许特定的密码，自定义的公钥(public keys)等等<sup>3</sup>。
</font> <p></p>
To connect to the daemon, any SSH client will do:
<p></p> <font color="green">
任意的的SSH客户端都可以连接上这个精灵进程(daemon)：
</font> <p></p>

-----------------------------------------------------------------------------------<br>
`$ ssh -p 8989 ferd@127.0.0.1`<br>
`Eshell Vx.x.x (abort with ^G)`<br>
`1>`<br>
-----------------------------------------------------------------------------------<br>
And with this you can interact with an Erlang installation without having it installed on the current machine. Just disconnecting from the SSH session (closing the terminal) will be enough to leave. Do not run functions such as q() or init:stop() , which will terminate the remote host. <sup>4</sup>
<p></p> <font color="green">
这样你就可以不用本地安装Erlang也使用远程的Erlang了。直接断开SSH session(关闭终端)就可以安全离开。但千万不能输出q()或init:stop()之类终结远程shell的命令<sup>4</sup>。
</font> <p></p>

If you have trouble connecting, you can add the -oLogLevel=DEBUG option to ssh to get debug output.
<p></p> <font color="green">
如果你还有是不能连接成功，你可以加一个选项 -oLogLevel=DEBUG 让把debug信息输出。
</font> <p></p>

[3] Complete instructions with all options to get this set up are available at
http://www.erlang.org/doc/man/ssh.html#daemon-3.<br>
[4] This is true for all methods of interacting with a remote Erlang node.
<p></p> <font color="green">

[注3]：关于所有选项的说明见：http://www.erlang.org/doc/man/ssh.html#daemon-3.<br>
[注4]：这种命令对所有连接远程Erlang节点的方法都不要用！
</font> <p></p>

