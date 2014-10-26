# Full Mailboxes
For loaded mailboxes, looking at large counters is the best way to do it. If there is one large mailbox, go investigate the process in the crash dump. Figure out if it’s happening because
it’s not matching on some message, or overload. If you have a similar node running, you
can log on it and go inspect it. If you find out many mailboxes are loaded, you may want
to use recon’s queue_fun.awk to figure out what function they’re running at the time of
the crash:<br>
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
