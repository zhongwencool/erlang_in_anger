# Profiling and Reduction Counts
To pin issues to specific pieces of Erlang code, as mentioned earlier, there are two main
approaches. One will be to do the old standard profiling routine, likely using one of the
following applications: <sup>2</sup>

<br>&emsp;• **eprof**, <sup>3</sup> the oldest Erlang profiler around. It will give general percentage values and
will mostly report in terms of time taken.
<br>&emsp;• **fprof**, <sup>4</sup> a more powerful replacement of eprof. It will support full concurrency and
generate in-depth reports. In fact, the reports are so deep that they are usually
considered opaque and hard to read.
<br>&emsp;• **eflame**, <sup>5</sup> the newest kid on the block. It generates flame graphs to show deep call
sequences and hot-spots in usage on a given piece of code. It allows to quickly find
issues with a single look at the final result.
<br>&emsp;It will be left to the reader to thoroughly read each of these application’s documentation.
The other approach will be to run recon:proc_window/3 as introduced in Subsection 5.2.1:

--------------------------------------------------<br>
`1> recon:proc_window(reductions, 3, 500).`<br>
`[{<0.46.0>,51728,`<br>
`[{current_function,{queue,in,2}},`<br>
`{initial_call,{erlang,apply,2}}]},`<br>
`{<0.49.0>,5728,`<br>
`[{current_function,{dict,new,0}},`<br>
`{initial_call,{erlang,apply,2}}]},`<br>
`{<0.43.0>,650,`<br>
`[{current_function,{timer,sleep,1}},`<br>
`{initial_call,{erlang,apply,2}}]}]`<br>
--------------------------------------------------<br>

<br>&emsp;The reduction count has a direct link to function calls in Erlang, and a high count is
usually the synonym of a high amount of CPU usage.
<br>&emsp;What’s interesting with this function is to try it while a system is already rather busy, <sup>6</sup>
with a relatively short interval. Repeat it many times, and you should hopefully see a
pattern emerge where the same processes (or the same kind of processes) tend to always
come up on top.
<br>&emsp;Using the code locations <sup>7</sup> and current functions being run, you should be able to identify
what kind of code hogs all your schedulers.








[2] All of these profilers work using Erlang tracing functionality with almost no restraint. They will have
an impact on the run-time performance of the application, and shouldn’t be used in production.<br>
[3] http://www.erlang.org/doc/man/eprof.html<br>
[4] http://www.erlang.org/doc/man/fprof.html<br>
[5] https://github.com/proger/eflame<br>
[6] See Subsection 5.1.2<br>
[7] Call recon:info(PidTerm, location) or process_info(Pid, current_stacktrace)
to get this information.<br>
