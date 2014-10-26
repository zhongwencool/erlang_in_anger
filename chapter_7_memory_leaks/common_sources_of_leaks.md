# Common Sources of Leaks
Whenever someone calls for help saying "oh no, my nodes are crashing", the first step is
always to ask for data. Interesting questions to ask and pieces of data to consider are:<br>
&emsp;• Do you have a crash dump and is it complaining about memory specifically? If not,
the issue may be unrelated. If so, go dig into it, it’s full of data.<br>
&emsp;• Are the crashes cyclical? How predictable are they? What else tends to happen at
around the same time and could it be related?<br>
&emsp;• Do crashes coincide with peaks in load on your systems, or do they seem to happen
at more or less any time? Crashes that happen especially during peak times are often
due to bad overload management (see Chapter 3). Crashes that happen at any time,
even when load goes down following a peak are more likely to be actual memory
issues.<br>
&emsp;If all of this seems to point towards a memory leak, install one of the metrics libraries
mentioned in Chapter 5 and/or recon and get ready to dive in. <sup>1</sup>
&emsp;The first thing to look at in any of these cases is trends. Check for all types of memory
using erlang:memory() or some variant of it you have in a library or metrics system. Check
for the following points:<br>
&emsp;• Is any type of memory growing faster than others?<br>
&emsp;• Is there any type of memory that’s taking the majority of the space available?<br>
&emsp;• Is there any type of memory that never seems to go down, and always up (other than
atoms)?<br>
Many options are available depending on the type of memory that’s growing.<br>
