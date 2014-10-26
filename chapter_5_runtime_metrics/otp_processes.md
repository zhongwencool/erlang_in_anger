# OTP Processes
When processes in question are OTP processes (most of the processes in a production
system should definitely be OTP processes), you instantly win more tools to inspect them.
In general the sys module <sup>23</sup> is what you want to look into. Read the documentation<br>
on it and you’ll discover why it’s so useful. It contains the following features for any OTP
process:<br>
&emsp• logging of all messages and state transitions, both to the shell or to a file, or even in
an internal buffer to be queried;<br>
&emsp;• statistics (reductions, message counts, time, and so on);<br>
&emsp;• fetching the status of a process (metadata including the state);<br>
&emsp;• fetching the state of a process (as in the #state{} record);<br>
&emsp;• replacing that state<br>
&emsp;• custom debugging functions to be used as callbacks<br>
It also provides functionality to suspend or resume process execution.<br>
I won’t go into a lot of details about these functions, but be aware that they exist.


[23] http://www.erlang.org/doc/man/sys.html
