# Exercises
## Review Questions
[1]. What are the two main approaches to pin issues about CPU usages?<br>
[2]. Name some of the profiling tools available. What approaches are preferable for production use? Why?<br>
[3]. Why can long scheduling monitors be useful to find CPU or scheduler over-consumption?
## Open-ended Questions
[1]. If you find that a process doing very little work with reductions ends up being scheduled for long periods of time, what can you guess about it or the code it runs?<br>
[2]. Can you set up a system monitor and then trigger it with regular Erlang code? Can
you use it to find out for how long processes seem to be scheduled on average? You
may need to manually start random processes from the shell that are more aggressive
in their work than those provided by the existing system.
