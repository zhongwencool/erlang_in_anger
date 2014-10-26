# Processes
There are a lot of different ways in which process memory can grow. Most interesting
cases will be related to a few common cases: process leaks (as in, you’re leaking processes),
specific processes leaking their memory, and so on. It’s possible there’s more than one
cause, so multiple metrics are worth investigating. Note that the process count itself is
skipped and has been covered before.
## Links and Monitors
Is the global process count indicative of a leak? If so, you may need to investigate unlinked processes, or peek inside supervisors’ children lists to see what may be weird-looking.<br>
&emsp;Finding unlinked (and unmonitored) processes is easy to do with a few basic commands:<br>
-----------------------------------------------------<br>
`1> [P || P <- processes(),`<br>
`[{_,Ls},{_,Ms}] <- [process_info(P, [links,monitors])],`<br>
`[]==Ls, []==Ms].`<br>
-----------------------------------------------------<br>
This will return a list of processes with neither. For supervisors, just fetching
supervisor: count_children(Supervi sorPidOrName) and seeing what looks normal can
be a good pointer.<br>
## Memory Used
The per-process memory model is briefly described in Subsection 7.3.2, but generally speaking, you can find which individual processes use the most memory by looking for their
memory attribute. You can look things up either as absolute terms or as a sliding window.
<br>&emsp;For memory leaks, unless you’re in a predictable fast increase, absolute values are usually those worth digging into first:<br>
-----------------------------------------------------<br>
`1> recon:proc_count(memory, 3).`<br>
`[{<0.175.0>,325276504,`<br>
`[myapp_stats,`<br>
`{current_function,{gen_server,loop,6}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.169.0>,73521608,`<br>
`[myapp_giant_sup,`<br>
`{current_function,{gen_server,loop,6}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.72.0>,4193496,`<br>
`[gproc,`<br>
`{current_function,{gen_server,loop,6}},`<br>
`{initial_call,{proc_lib,init_p,5}}]}]`<br>
-----------------------------------------------------<br>
&emsp;Attributes that may be interesting to check other than memory may be any other fields in Subsection 5.2.1, including **message_queue_len**, but memory will usually encompass all other types.
## Garbage Collections
It is very well possible that a process uses lots of memory, but only for short periods of time.
For long-lived nodes with a large overhead for operations, this is usually not a problem, but
whenever memory starts being scarce, such spiky behaviour might be something you want
to get rid of.<br>
Monitoring all garbage collections in real-time from the shell would be costly. Instead,
setting up Erlang’s system monitor <sup>7</sup> might be the best way to go at it.
Erlang’s system monitor will allow you to track information such as long garbage collection periods and large process heaps, among other things. A monitor can temporarily
be set up as follows:<br>
-----------------------------------------------------<br>
`1> erlang:system_monitor().`<br>
`undefined`<br>
`2> erlang:system_monitor(self(), [{long_gc, 500}]).`<br>
`undefined`<br>
`3> flush().`<br>
`Shell got {monitor,<4683.31798.0>,long_gc,`<br>
`[{timeout,515},`<br>
`{old_heap_block_size,0},`<br>
`{heap_block_size,75113},`<br>
`{mbuf_size,0},`<br>
`{stack_size,19},`<br>
`{old_heap_size,0},`<br>
`{heap_size,33878}]}`<br>
`5> erlang:system_monitor(undefined).`<br>
`{<0.26706.4961>,[{long_gc,500}]}`<br>
`6> erlang:system_monitor().`<br>
`undefined`<br>
-----------------------------------------------------<br>
&emsp;The first command checks that nothing (or nobody else) is using a system monitor yet — you don’t want to take this away from an existing application or coworker.
<br>&emsp;The second command will be notified every time a garbage collection takes over 500 milliseconds. The result is flushed in the third command. Feel free to also check for
**{large_heap, NumWords}** if you want to monitor such sizes. Be careful to start with large
values at first if you’re unsure. You don’t want to flood your process’ mailbox with a bunch
of heaps that are 1-word large or more, for example.
<br>&emsp;Command 5 unsets the system monitor (exiting or killing the monitor process also frees
it up), and command 6 validates that everything worked.
<br>&emsp;You can then find out if such monitoring messages tend to coincide with the memory
increases that seem to result in leaks or overuses, and try to catch culprits before things
are too bad. Quickly reacting and digging into the process (possibly with recon:info/1)
may help find out what’s wrong with the application.

[7] http://www.erlang.org/doc/man/erlang.html#system_monitor-2<br>
