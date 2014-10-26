# Too Many Ports
Similarly to the process count, the port count is simple and mostly useful when you know
your usual values <sup>7</sup>.<br>
A high count may be the result of overload, Denial of Service attacks, or plain old
resource leaks. Looking at the type of port leaked (TCP, UDP, or files) can also help reveal if there was contention on specific resources, or if the code using them is just wrong.
<p></p>
[7] See subsection 5.1.4 for details
