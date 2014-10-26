# Finding Fragmentation
The **recon_alloc** module was developed specifically to detect and help point towards the
resolution of such issues.<br>
&emsp;Given how rare this type of issue has been so far over the community (or happened
without the developers knowing what it was), only broad steps to detect things are defined.
They’re all vague and require the operator’s judgement.
## Check Allocated Memory
Calling **recon_alloc:memory/1** will report various memory metrics with more flexibility
than **erlang:memory/0**. Here are the possibly relevant arguments:<br>
<br>&emsp;1. call recon_alloc:memory(usage) . This will return a value between 0 and 1 representing a percentage of memory that is being actively used by Erlang terms versus
the memory that the Erlang VM has obtained from the OS for such purposes. If the
usage is close to 100%, you likely do not have memory fragmentation issues. You’re
just using a lot of it.
<br>&emsp;2. check if recon_alloc:memory(allocated) matches what the OS reports. <sup>12</sup> It should
match it fairly closely if the problem is really about fragmentation or a memory leak
from Erlang terms.
That should confirm if memory seems to be fragmented or not.
## Find the Guilty Allo cator
Call **recon_alloc:memory**(allocated_types) to see which type of util allocator (see Section 7.3.2) is allocating the most memory. See if one looks like an obvious culprit when you
compare the results with erlang:memory() .
Try **recon_alloc:fragmentation(current) **. The resulting data dump will show different allocators on the node with various usage ratios. <sup>13</sup>
<br>&emsp;If you see very low ratios, check if they differ when calling recon_alloc:fragmentation(max) ,
which should show what the usage patterns were like under your max memory load.
<br>&emsp;If there is a big difference, you are likely having issues with memory fragmentation for
a few specific allocator types following usage spikes.

[12] You can call **recon_alloc:set_unit(Type)** to set the values reported by recon_alloc in bytes,
kilobytes, megabytes, or gigabytes<br>
[13] More information is available at http://ferd.github.io/recon/recon_alloc.html
