# Code
The code on an Erlang node is loaded in memory in its own area, and sits there until it is
garbage collected. Only two copies of a module can coexist at one time, so looking for very
large modules should be easy-ish.<br>
&emsp;If none of them stand out, look for code compiled with HiPE <sup>5</sup>. HiPE code, unlike
regular BEAM code, is native code and cannot be garbage collected from the VM when
new versions are loaded. Memory can accumulate, usually very slowly, if many or large
modules are native-compiled and loaded at run time.<br>
&emsp;Alternatively, you may look for weird modules you didn’t load yourself on the node and panic if someone got access to your system!


[5] http://www.erlang.org/doc/man/HiPE_app.html<br>
