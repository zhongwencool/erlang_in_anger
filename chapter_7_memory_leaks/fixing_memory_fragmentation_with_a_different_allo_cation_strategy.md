# Fixing Memory Fragmentation with a Different Allo cation Strategy
Tweaking your VM’s options for memory allocation may help.
<br>&emsp;You will likely need to have a good understanding of what your type of memory load and
usage is, and be ready to do a lot of in-depth testing. The recon_alloc module contains
a few helper functions to provide guidance, and the module’s documentation <sup>22</sup> should be
read at this point.
<br>&emsp;You will need to figure out what the average data size is, the frequency of allocation and
deallocation, whether the data fits in mbcs or sbcs, and you will then need to try playing
with a bunch of the options mentioned in recon_alloc, try the different strategies, deploy
them, and see if things improve or if they impact times negatively.
<br>&emsp;This is a very long process for which there is no shortcut, and if issues happen only
every few months per node, you may be in for the long haul.


[22] http://ferd.github.io/recon/recon_alloc.html
