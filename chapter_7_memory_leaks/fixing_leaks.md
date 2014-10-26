# Fixing Leaks
Once you’ve established you’ve got a binary memory leak using **recon:bin_leak(Max)** , it
should be simple enough to look at the top processes and see what they are and what kind
of work they do.
<br>&emsp;Generally, refc binaries memory leaks can be solved in a few different ways, depending
on the source:
<br>&emsp;• call garbage collection manually at given intervals (icky, but somewhat efficient);
<br>&emsp;• stop using binaries (often not desirable);
<br>&emsp;• use** binary:copy/1-2**<sup>10</sup> if keeping only a small fragment (usually less than 64 bytes)
of a larger binary;<sup>11</sup>
<br>&emsp;• move work that involves larger binaries to temporary one-off processes that will die
when they’re done (a lesser form of manual GC!);
<br>&emsp;• or add hibernation calls when appropriate (possibly the cleanest solution for inactive
processes).
<br>&emsp;The first two options are frankly not agreeable and should not be attempted before all
else failed. The last three options are usually the best ones to be used.

## Routing Binaries
There’s a specific solution for a specific use case some Erlang users have reported. The
problematic use case is usually having a middleman process routing binaries from one
process to another one. That middleman process will therefore acquire a reference to every
binary passing through it and risks being a common major source of refc binaries leaks.
The solution to this pattern is to have the router process return the pid to route to and
let the original caller move the binary around. This will make it so that only processes that
do need to touch the binaries will do so.
<br>&emsp;A fix for this can be implemented transparently in the router’s API functions, without
any visible change required by the callers.

[10] http://www.erlang.org/doc/man/binary.html#copy-1
[11] It might be worth copying even a larger fragment of a refc binary. For example, copying 10 megabytes
off a 2 gigabytes binary should be worth the short-term overhead if it allows the 2 gigabytes binary to be
garbage-collected while keeping the smaller fragment longer.
