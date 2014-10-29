# Nothing in Particular
If nothing seems to stand out in the preceding material, binary leaks (Section 7.2) and
memory fragmentation (Section 7.3) may be the culprits. If nothing there fits either, it’s
possible a C driver, NIF, or even the VM itself is leaking. Of course, a possible scenario is that load on the node and memory usage were proportional, and nothing specifically ended
up being leaky or modified. The system just needs more resources or nodes.

<p></p> <font color="green">
&emsp;如果前面所说的部分都没有看出问题，那么章节7.2的二进制泄露和章节7.3的内存碎片可能是罪魁祸首。如果这也没有找出问题，变有可能是c dirver,NIF或VM自身存在溢出。当然，也有可能是节点上的负载和内存的使用比例太高，并没有特别的溢出或修改，只是系统需要更多的资源或节点。
</font> <p></p>

