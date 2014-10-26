# Digging In
Whenever some ’in the large’ view (or logging, maybe) has pointed you towards a potential
cause for an issue you’re having, it starts being interesting to dig around with a purpose. Is a process in a weird state? Maybe it needs tracing <sup>16</sup>! Tracing is great whenever you have a specific function call or input or output to watch for, but often, before getting there, a lot more digging is required.<br>
Outside of memory leaks, which often need their own specific techniques and are discussed in Chapter 7, the most common tasks are related to processes, and ports (file descriptors and sockets).
<p></p> <font color="green">



[16] See Chapter 9<br>
[17] In cases where processes contain sensitive information, data can be forced to be kept private by calling process_flag(sensitive, true) <br>
[18] For all options, look at http://www.erlang.org/doc/man/erlang.html#process_info-2<br>
[19] See http://www.erlang.org/course/advanced.html#dict and http://ferd.ca/on-the-use-of-the-processdictionary-in-erlang.html<br>
[20] See http://learnyousomeerlang.com/building-otp-applications#the-application-behaviour and http://erlang.org/doc/apps/stdlib/io_protocol.html for more details.

<p></p> <font color="green">
</font> <p></p>

