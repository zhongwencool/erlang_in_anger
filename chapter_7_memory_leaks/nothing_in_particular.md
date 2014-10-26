# Nothing in Particular
If nothing seems to stand out in the preceding material, binary leaks (Section 7.2) and
memory fragmentation (Section 7.3) may be the culprits. If nothing there fits either, it’s
possible a C driver, NIF, or even the VM itself is leaking. Of course, a possible scenario is that load on the node and memory usage were proportional, and nothing specifically ended
up being leaky or modified. The system just needs more resources or nodes.
