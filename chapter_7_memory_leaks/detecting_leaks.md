# Detecting Leaks
Detecting leaks for reference-counted binaries is easy enough: take a measure of all of
each process’ list of binary references (using the binary attribute), force a global garbage collection, take another snapshot, and calculate the difference.
<br>&emsp;This can be done directly with recon:bin_leak(Max) and looking at the node’s total memory before and after the call:<br>
--------------------------------------------------------<br>
`1> recon:bin_leak(5).`<br>
`[{<0.4612.0>,-5580,`<br>
`[{current_function,{gen_fsm,loop,7}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.17479.0>,-3724,`<br>
`[{current_function,{gen_fsm,loop,7}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.31798.0>,-3648,`<br>
`[{current_function,{gen_fsm,loop,7}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.31797.0>,-3266,`<br>
`[{current_function,{gen,do_call,4}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.22711.1>,-2532,`<br>
`[{current_function,{gen_fsm,loop,7}},`<br>
`{initial_call,{proc_lib,init_p,5}}]}]`<br>
--------------------------------------------------------<br>

<br>&emsp;This will show how many individual binaries were held and then freed by each process
as a delta. The value -5580 means there were 5580 fewer refc binaries after the call than
before.
<br>&emsp;It is normal to have a given amount of them stored at any point in time, and not all
numbers are a sign that something is bad. If you see the memory used by the VM go down
drastically after running this call, you may have had a lot of idling refc binaries.
<br>&emsp;Similarly, if you instead see some processes hold impressively large numbers of them <sup>9</sup>,
that might be a good sign you have a problem.
<br>&emsp;You can further validate the top consumers in total binary memory by using the special
**binary_memory** attribute supported in **recon**:
--------------------------------------------------------<br>
`1> recon:proc_count(binary_memory, 3).`<br>
`[{<0.169.0>,77301349,`<br>
`[app_sup,`<br>
`{current_function,{gen_server,loop,6}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.21928.1>,9733935,`<br>
`[{current_function,{erlang,hibernate,3}},`<br>
`{initial_call,{proc_lib,init_p,5}}]},`<br>
`{<0.12386.1172>,7208179,`<br>
`[{current_function,{erlang,hibernate,3}},`<br>
`{initial_call,{proc_lib,init_p,5}}]}]`<br>
--------------------------------------------------------<br>
<br>&emsp;This will return the N top processes sorted by the amount of memory the refc binaries reference to hold, and can help point to specific processes that hold a few large binaries, instead
of their raw amount. You may want to try running this function before recon:bin_leak/1,
given the latter garbage collects the entire node first.

[9] We’ve seen some processes hold hundreds of thousands of them during leak investigations at Heroku!

