# Exercises

## Review Questions
[1]. Why is debugger use generally limited on Erlang?<br>
[2]. What are the options you can use to trace OTP processes?<br>
[3]. What determines whether a given set of functions or processes get traced?<br>
[4]. How can you stop tracing with recon_trace? With other tools?<br>
[5]. How can you trace non-exported function calls?<br>
## Open-ended Questions
[1]. When would you want to move time stamping of traces to the VM’s trace mechanisms
directly? What would be a possible downside of doing this?<br>
[2]. Imagine that traffic sent out of a node does so over SSL, over a multi-tenant system.
However, due to wanting to validate data sent (following a customer complain), you
need to be able to inspect what was seen clear text. Can you think up a plan to be
able to snoop in the data sent to their end through the ssl socket, without snooping
on the data sent to any other customer?<br>
## Hands-On
Using the code at https://github.com/ferd/recon_demo (these may require a decent understanding of the code there):<br>
[1]. Can chatty processes (council_member) message themselves? (hint: can this work
with registered names? Do you need to check the chattiest process and see if it messages
itself ?)<br>
[2]. Can you estimate the overall frequency at which messages are sent globally?<br>
[3]. Can you crash a node using any of the tracing tools? (hint: dbg makes it easier due
to its greater flexibility)<br>
