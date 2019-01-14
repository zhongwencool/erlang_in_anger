# Atom
# 原子
*Don’t use dynamic atoms!* Atoms go in a global table and are cached forever. Look
for places where you call **erlang:binary_to_term/1** and** erlang:list_to_atom/1**, and
consider switching to safer variants (**erlang:binary_to_term(Bin, [safe])** and
**erlang:list_to_existing_atom/1**).<br>
&emsp;If you use the xmerl library that ships with Erlang, consider open source alternatives <sup>2</sup>or figuring the way to add your own SAX parser that can be safe <sup>3</sup>.<br>
<p></p> <font color="green">
&emsp;*不要动态生成 atom！*，Atom 都是存在一个全局表中，并会被永远保存下来。找到代码中使用**erlang:binary_to_term/1**和** erlang:list_to_atom/1**的地方，考虑用更安全的版本(**erlang:binary_to_term(Bin, [safe])**和**erlang:list_to_existing_atom/1**)代替它们.<br>
</font> <p></p>
<p></p> <font color="green">
&emsp;如果你在使用 Erlang 自带的 xmerl 库来解析 XML, 那么请再考虑下其它开源替代方案<sup>2</sup>或者自行开发一个安全的 SAX 解析器<sup>3</sup>。<br>
</font> <p></p>

&emsp;If you do none of this, consider what you do to interact with the node. One specific
case that bit me in production was that some of our common tools used random names to
connect to nodes remotely, or generated nodes with random names that connected to each
other from a central server. <sup>4</sup> Erlang node names are converted to atoms, so just having this was enough to slowly but surely exhaust space on atom tables. Make sure you generate
them from a fixed set, or slowly enough that it won’t be a problem in the long run.<br>
<p></p> <font color="green">
&emsp;如果你上面的原则都没有违反，那么有可能是与节点交互时的问题。我在生产环境中就曾遇到过这样的情况：我们一些常用的工具使用了随机名字来连接远程节点，或是通过中央服务器来连接有着随机生成名字的节点<sup>4</sup>。 注意 Erlang 节点名称会转化为 atom，所以这样做就耗尽 atom 表(虽然过程很慢)。请确保节点名是相对固定的，或是随机生成的足够慢以保证长期运行的稳定性。
</font> <p></p>


[2] I don’t dislike exml or erlsom<br>
[3] See Ulf Wiger at http://erlang.org/pipermail/erlang-questions/2013-July/074901.html<br>
[4] This is a common approach to figuring out how to connect nodes together: have one or two central nodes with fixed names, and have every other one log to them. Connections will then propagate automatically.
<p></p> <font color="green">
[注2] 比方说 exml 或 erlsom<br>
[注3] 可以参考 Ulf Wiger 的这个回答： http://erlang.org/pipermail/erlang-questions/2013-July/074901.html<br>
[注4] 把节点连接在一起的常用方法：有1到2个固定名字的中央节点，让其它的节点都连接上来。连接将会自动传播开来。
</font> <p></p>
