# Can’t Allocate Memory
These are by far the most common types of crashes you are likely to see. There’s so much to
cover, that Chapter 7 is dedicated to understanding them and doing the required debugging
on live systems.<br>
&emsp;In any case, the crash dump will help figure out what the problem was after the fact.
The process mailboxes and individual heaps are usually good indicators of issues. If you’re
running out of memory without any mailbox being outrageously large, look at the processes
heap and stack sizes as returned by the recon script.<br>
&emsp;In case of large outliers at the top, you know some restricted set of processes may be
eating up most of your node’s memory. In case they’re all more or less equal, see if the
amount of memory reported sounds like a lot.<br>
&emsp;If it looks more or less reasonable, head towards the "Memory" section of the dump
and check if a type (ETS or Binary, for example) seems to be fairly large. They may point
towards resource leaks you hadn’t expected.<br>
