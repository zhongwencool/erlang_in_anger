# Suspended Ports
An interesting part of system monitors that didn’t fit anywhere but may have to do with
scheduling is regarding ports. When a process sends too many message to a port and the
port’s internal queue gets full, the Erlang schedulers will forcibly de-schedule the sender
until space is freed. This may end up surprising a few users who didn’t expect that implicit
back-pressure from the VM.
This kind of event can be monitored by passing in the atom **busy_port** to the system
monitor. Specifically for clustered nodes, the atom **busy_dist_port** can be used to find
when a local process gets de-scheduled when contacting a process on a remote node whose
inter-node communication was handled by a busy port.
If you find out you’re having problems with these, try replacing your sending functions
where in critical paths with **erlang:port_command(Port, Data, [nosuspend])** for ports,
and **erlang:send(Pid, Msg, [nosuspend])** for messages to distributed processes. They
will then tell you when the message could not be sent and you would therefore have been
descheduled.
