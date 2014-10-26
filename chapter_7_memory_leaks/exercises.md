# Exercises
## Review Questions
[1]. Name some of the common sources of leaks in Erlang programs.<br>
[2]. What are the two main types of binaries in Erlang?<br>
[3]. What could be to blame if no specific data type seems to be the source of a leak?<br>
[4]. If you find the node died with a process having a lot of memory, what could you do
to find out which one it was?<br>

[5]. How could code itself cause a leak?<br>
[6]. How can you find out if garbage collections are taking too long to run?<br>
## Open-ended Questions
[1]. How could you verify if a leak is caused by forgetting to kill processes, or by processes
using too much memory on their own?<br>
[2]. A process opens a 150MB log file in binary mode to go extract a piece of information
from it, and then stores that information in an ETS table. After figuring out you
have a binary memory leak, what should be done to minimize binary memory usage
on the node?<br>
[3]. What could you use to find out if ETS tables are growing too fast?<br>
[4]. What steps should you go through to find out that a node is likely suffering from
fragmentation? How could you disprove the idea that is could be due to a NIF or
driver leaking memory?<br>
[5]. How could you find out if a process with a large mailbox (from reading **message_queue_len**)<br>
seems to be leaking data from there, or never handling new messages?<br>
[6]. A process with a large memory footprint seems to be rarely running garbage collections. What could explain this?<br>
[7]. When should you alter the allocation strategies on your nodes? Should you prefer to
tweak this, or the way you wrote code?<br>
## Hands-On
[1]. Using any system you know or have to maintain in Erlang (including toy systems),
can you figure out if there are any binary memory leaks on there?<br>
