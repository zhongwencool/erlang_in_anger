# Full Mailboxes
For loaded mailboxes, looking at large counters is the best way to do it. If there is one large mailbox, go investigate the process in the crash dump. Figure out if it’s happening because
it’s not matching on some message, or overload. If you have a similar node running, you
can log on it and go inspect it. If you find out many mailboxes are loaded, you may want
to use recon’s **queue_fun.awk** to figure out what function they’re running at the time of
the crash:<br>
<p></p> <font color="green">
&emsp;对于大量累积的信箱，最好的方法就是查看消息数量。如果发现一个信箱特别多的消息，那么就去在crash dump里面调查这个进程，看看他是不是因为没有匹配到一些消息或超负荷了。如果你还有一个类似的在运行状态的节点，你就可以登录上去，检查它。如果你发现很多信箱都超载了。你就可以使用recon’s **queue_fun.awk**来找出它们现在crash时运行了什么函数。
</font> <p></p>

-------------------------------------------------------------------<br>
`1 $ awk -v threshold=10000 -f queue_fun.awk /path/to/erl_crash.dump`<br>
`2 MESSAGE QUEUE LENGTH: CURRENT FUNCTION`<br>
`3 ======================================`<br>
`4 10641: io:wait_io_mon_reply/2`<br>
`5 12646: io:wait_io_mon_reply/2`<br>
`6 32991: io:wait_io_mon_reply/2`<br>
`7 2183837: io:wait_io_mon_reply/2`<br>
`8 730790: io:wait_io_mon_reply/2`<br>
`9 80194: io:wait_io_mon_reply/2`<br>
`10 ...`<br>
-------------------------------------------------------------------<br>
&emsp;This one will run over the crash dump and output all of the functions scheduled to run
for processes with at least 10000 messages in their mailbox. In the case of this run, the
script showed that the entire node was locking up waiting on IO for io:format/2 calls, for
example.

<p></p> <font color="green">
&emsp;这可以遍历crash dump然后把信箱超过10000消息的进程使用的函数都打印出来。比如上面这例子就显示出整个节点都被锁住并等待IO使用io:format/2调用。
</font> <p></p>
