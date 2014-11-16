# Atom
# 原子
*Don’t use dynamic atoms!* Atoms go in a global table and are cached forever. Look
for places where you call **erlang:binary_to_term/1** and** erlang:list_to_atom/1**, and
consider switching to safer variants (**erlang:binary_to_term(Bin, [safe])** and
**erlang:list_to_existing_atom/1**).<br>
&emsp;If you use the xmerl library that ships with Erlang, consider open source alternatives <sup>2</sup>or figuring the way to add your own SAX parser that can be safe <sup>3</sup>.<br>
<p></p> <font color="green">
&emsp;*不要动态生成atoms！*，Atoms都是存在一个全局表中，并会被永远保存下来。找到代码中使用**erlang:binary_to_term/1**和** erlang:list_to_atom/1**的地方，考虑用更安全的它们的变体函数(**erlang:binary_to_term(Bin, [safe])**和**erlang:list_to_existing_atom/1**)代替它们.<br>
</font> <p></p>
<p></p> <font color="green">
&emsp;如果你使用xmerl库来过度(ships with)Erlang,那么请再考虑下其它开源替代方案<sup>2</sup>或找到一种可以安全增添你自己的SAX解析<sup>3</sup>。<br>
</font> <p></p>

&emsp;If you do none of this, consider what you do to interact with the node. One specific
case that bit me in production was that some of our common tools used random names to
connect to nodes remotely, or generated nodes with random names that connected to each
other from a central server. <sup>4</sup> Erlang node names are converted to atoms, so just having this was enough to slowly but surely exhaust space on atom tables. Make sure you generate
them from a fixed set, or slowly enough that it won’t be a problem in the long run.<br>
<p></p> <font color="green">
&emsp;如果你上面的原则都没有违反，细想下你在与节点交互时做了什么？我在实际生产环境中遇到的一个比较典型的情况：我们的一些常用工具使用了随机名字来连接远程的节点，随机生成的名字的节点通过中央服务器来相互连接<sup>4</sup>。 Erlang节点名称会转化为atoms，所以这样做就耗尽atom的表(虽然过程很慢)。请确保你通过fixed set来生成的，或慢得足够保证它不会在长期运行时存在耗尽问题。
</font> <p></p>


[2] I don’t dislike exml or erlsom<br>
[3] See Ulf Wiger at http://erlang.org/pipermail/erlang-questions/2013-July/074901.html<br>
[4] This is a common approach to figuring out how to connect nodes together: have one or two central nodes with fixed names, and have every other one log to them. Connections will then propagate automatically.
<p></p> <font color="green">
[注2] 我并不讨厌 exml 或 erlsom<br>
[注3] 查看 Ulf Wiger ： http://erlang.org/pipermail/erlang-questions/2013-July/074901.html<br>
[注4] 把节点连接在一起的常用方法：有1到2个固定名字的中央节点，让其它的节点都连接上来。连接将会自动传播开来。
</font> <p></p>























