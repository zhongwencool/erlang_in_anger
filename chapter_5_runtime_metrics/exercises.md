# Exercises
## Review Questions
[1]. What kind of values are reported for Erlang’s memory?<br>
[2]. What’s a valuable process-related metric for a global view?<br>
[3]. What’s a port, and how should it be monitored globally?<br>
[4]. Why can’t you trust top or htop for CPU usage with Erlang systems? What’s the
alternative?<br>
[5]. Name two types of signal-related information available for processes<br>
[6]. How can you find what code a specific process is running?<br>
[7]. What are the different kinds of memory information available for a specific process?<br>
[8]. How can you know if a process is doing a lot of work?<br>
[9]. Name a few of the values that are dangerous to fetch when inspecting processes in a
production system.<br>
[10]. What are some features provided to OTP processes through the sys module?<br>
[11]. What kind of values are available when inspecting inet ports?<br>
[12]. How can you find the type of a port (Files, TCP, UDP)?<br>
<p></p> <font color="green">
</font> <p></p>
## Open-ended Questions
[1]. Why do you want a long time window available on global metrics?<br>
[2]. Which would be more appropriate between recon:proc_count/2 and recon:proc_window/3
to find issues with:<br>
(a) Reductions<br>
(b) Memory<br>
(c) Message queue length<br>
[3]. How can you find information about who is the supervisor of a given process?<br>
[4]. When should you use recon:inet_count/2? recon:inet_window/3?<br>
[5]. What could explain the difference in memory reported by the operating system and
the memory functions in Erlang?<br>
[6]. Why is it that Erlang can sometimes look very busy even when it isn’t?<br>
[7]. How can you find what proportion of processes on a node are ready to run, but can’t
be scheduled right away?<br>
## Hands-On
Using the code at https://github.com/ferd/recon_demo:<br>
[1]. What’s the system memory?<br>
[2]. Is the node using a lot of CPU resources?<br>
[3]. Is any process mailbox overflowing?<br>
[4]. Which chatty process (council_member) takes the most memory?<br>
[5]. Which chatty process is eating the most CPU?<br>
[6]. Which chatty process is consuming the most bandwidth?<br>
[7]. Which chatty process sends the most messages over TCP? The least?<br>
[8]. Can you find out if a specific process tends to hold multiple connections or file descriptors open at the same time on a node?<br>
[9]. Can you find out which function is being called by the most processes at once on the
node right now?<br>
