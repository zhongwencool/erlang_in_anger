# Atom
*Don’t use dynamic atoms!* Atoms go in a global table and are cached forever. Look
for places where you call **erlang:binary_to_term/1** and** erlang:list_to_atom/1**, and
consider switching to safer variants (**erlang:binary_to_term(Bin, [safe])** and
erlang:list_to_existing_atom/1).<br>
&emsp;If you use the xmerl library that ships with Erlang, consider open source alternatives <sup>2</sup>or figuring the way to add your own SAX parser that can be safe <sup>3</sup>.<br>
&emsp;If you do none of this, consider what you do to interact with the node. One specific
case that bit me in production was that some of our common tools used random names to
connect to nodes remotely, or generated nodes with random names that connected to each
other from a central server. <sup>4</sup> Erlang node names are converted to atoms, so just having this
was enough to slowly but surely exhaust space on atom tables. Make sure you generate
them from a fixed set, or slowly enough that it won’t be a problem in the long run.<br>
[1] See Chapter 4 if you need help to connect to a running node<br>
[2] I don’t dislike exml or erlsom<br>
[3] See Ulf Wiger at http://erlang.org/pipermail/erlang-questions/2013-July/074901.html<br>
[4] This is a common approach to figuring out how to connect nodes together: have one or two central nodes with fixed names, and have every other one log to them. Connections will then propagate automatically.
