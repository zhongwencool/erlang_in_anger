# OTP Releases

OTP releases are not a lot harder to understand than most OTP applications you’ll encounter in the wild. A release is a set of OTP applications packaged in a production-ready manner so it boots and shuts down without needing to manually call application:start/2 for any app.
<p></p>
<font color="green">
OTP releases 理解起来并不比你遇到的OTP applications难。一个release就是使用量产(production-ready)方式打包的一组OTP applications,所以它的启动和关闭都不需要手动去调用application:start/2.
</font>
<p></p>
Of course there’s a bit more to releases than that, but generally, the same discovery process used for individual OTP applications will be applicable here.
You’ll usually have a file similar to the configuration files used by systools or reltool, which will state all applications part of the release and a few <sup>8</sup> options regarding their packaging.
<p></p>
<font color="green">
当然，release 还有做得更多一点，但通常来讲， 你在OTP applications中的发现同样适用release.
你通常会配置一些类似的文件，供systools或reltool使用(记录所有applications关于release的部分和一些选项<sup>8</sup>)。
</font>
To understand them, I recommend reading existing documentation on them. If you’re lucky, the project may be using relx <sup>9</sup>, an easier tool that was officially released in early 2014.
<p></p>
<font color="green">
为了理解他们，我推荐看看官方已有的文档，如果你幸运的话，项目可能会使用relx,一个官方在2014年上半年推出的非常简单的工具.
</font>


[8] A lot <br>
[9] https://github.com/erlware/relx/wiki
<p></p>
<font color="green">
[注8：很多]<br>
[注9：https://github.com/erlware/relx/wiki]
</font>
