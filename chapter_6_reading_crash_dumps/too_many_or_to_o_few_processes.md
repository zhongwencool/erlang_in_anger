# Too Many (or to o few) Processes
The process count is mostly useful when you know your node’s usual average count <sup>6</sup>, in
order to figure if it’s abnormal or not.<br>
&emsp;A count that is higher than normal may reveal a specific leak or overload, depending
on applications.<br>
If the process count is extremely low compared to usual, see if the node terminated with
a slogan like:<br>
------------------------------------------------------<br>
`<br> Kernel pid terminated (application_controller)`<br>
`<br> ({application_terminated, <AppName>, shutdown})`<br>
------------------------------------------------------<br>
&emsp;In such a case, the issue is that a specific application (<AppName>) has reached its
maximal restart frequency within its supervisors, and that prompted the node to shut down. Error logs that led to the cascading failure should be combed over to figure things
out.
<p></p>
[6] See subsection 5.1.3 for details<br>
