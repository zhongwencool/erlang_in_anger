# Chapter 6 Reading Crash Dumps
Whenever an Erlang no de crashes, it will generate a crash dump <sup>1</sup>.<br>
The format is mostly documented in Erlang’s official documentation <sup>2</sup>, and anyone willing to dig deeper inside of it will likely be able to figure out what data means by looking
at that documentation. There will be specific data that is hard to understand without also
understanding the part of the VM they refer to, but that might be too complex for this
document.<br>
The crash dump is going to be named **erl_crash.dump** and be located wherever the Erlang process was running by default. This behaviour (and the file name) can be overridden
by specifying the **ERL_CRASH_DUMP** environment variable <sup>3</sup>.
<p></p>
[1] If it isn’t killed by the OS for violating ulimits while dumping or didn’t segfault.<br>
[2] http://www.erlang.org/doc/apps/erts/crash_dump.html<br>
[3] Heroku’s Routing and Telemetry teams use the heroku_crashdumps app to set the path and name of the crash dumps. It can be added to a project to name the dumps by boot time and put them in a pre-set location
