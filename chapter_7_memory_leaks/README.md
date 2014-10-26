# Chapter 7 Memory Leaks
There are truckloads of ways for an Erlang no de to bleed memory. They go from extremely
simple to astonishingly hard to figure out (fortunately, the latter typ e is also rarer), and it’s p ossible you’ll never encounter any problem with them.<br>
&emsp;You will find out ab out memory leaks in two ways:<br>
<p></p>
&emsp;1. A crash dump (see Chapter 6);<br>
&emsp;2. By finding a worrisome trend in the data you are monitoring.<br>
<p></p>
&emsp;This chapter will mostly focus on the latter kind of leak, because they’re easier to
investigate and see grow in real time. We will focus on finding what is growing on the
node and common remediation options, handling binary leaks (they’re a special case), and
detecting memory fragmentation.
