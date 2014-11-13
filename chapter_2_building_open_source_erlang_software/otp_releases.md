# OTP Releases
For releases, the structure should be a bit different <sup>4</sup>. Releases are collections of applications, and their structures should reflect that.<br>
Instead of having a top-level app, applications should be nested one level deeper and divided into two categories: apps and deps. The apps directory contains your applications’ source code (say, internal business code), and the deps directory contains independently managed dependency applications.
 <p></p>
 <font color="green">
&emsp;对于Releases,结构会有一点小小的变化。Releases是applicaitons的集合，他们的结构也应当反映出这种集合特性。<br>
&emsp;一个applications应当拆分成apps和deps两类，deps里的依赖项又有自己的子deps的嵌套结构，而不是那些自上而下的app结构(所有依赖项都放在一个deps里面的形式是不可取的)， apps文件夹里面包括你自己applicaitons的源文件(内部业务代码)，deps文件夹包括独立管理依赖applications。<br>
</font>
---------------------------------------------------------------------------------<br>
1 `apps/`<br>
2 `doc/`<br>
3 `deps/`<br>
4 `LICENSE.txt`<br>
5 `README.md`<br>
6 `rebar.config`<br>
---------------------------------------------------------------------------------<br>
<p></p>
This structure lends itself to generating releases. Tools such as Systool and Reltool have been covered before <sup>5</sup>, and can allow the user plenty of power. An easier tool that recently appeared is relx <sup>6</sup>.<br>
A relx configuration file for the directory structure above would look like:
<p></p>
<font color="green">
&emsp;这种结构有助于自动生成releases。Systool,Reltool等工具以前都会支持这种结构，用起来非常给力。相对容易上手的还有最近推出的一个简单点的工具：relx<sup>6</sup>。<br>
&emsp;relx的配置文件格式如下：<br>
----------------------------------------------------------------------------------<br>
1 `{paths, ["apps", "deps"]}.`<br>
2 `{include_erts, false}. % will use currently installed Erlang`<br>
3 `{default_release, demo, "1.0.0"}.`<br>
4 <br>
5 `{release, {demo, "1.0.0"},`<br>
6 `[members,`<br>
7 `feedstore,`<br>
8 `...`<br>
9 `recon]}.`<br>
----------------------------------------------------------------------------------<br>
</font>
<p></p>
Calling **./relx** (if the executable is in the current directory) will build a release, to be found in the _rel/ directory.<br>
If you really like using rebar, you can build a release as part of the project’s compilation by using a rebar hook in rebar.config:
<p></p>
<font color="green">
&emsp;你可以调用**./relx**(在当前文件夹下调用)来创建一个release,就能在 _rel/文件夹来找到这个release.<br>
&emsp;如果你更喜欢使用rebar创建release，可以通过rebar.config里面的rebar hook 来把创建release，这装会把构建release工作作为项目编译的一部分。<br>
</font>
----------------------------------------------------------------------------------<br>
1 `{post_hooks,[{compile, "./relx"}]}.`<br>
----------------------------------------------------------------------------------<br>
<p></p>
And every time rebar compile will be called, the release will be generated.
<p></p><font color="green">
每次运行rebar compile时，都会新建一个release.
</font>
<p></p>
[4] I say should because many Erlang developers put their final system under a single top-level application (in src) and a bunch of follower ones as dependencies (in deps),
which is less than ideal for distribution purposes and conflicts with assumptions on directory structures made by OTP.<br>
 People who do that tend to build from source on the production servers and run custom commands to boot their applications.<br>
[5] http://learnyousomeerlang.com/release-is-the-word<br>
[6] https://github.com/erlware/relx/wiki
<p></p>

<font color="green">
[注4]：我说应当(should),是因为大量的Erlang开发者都会把最终的系统放在src做成单一的应用(a single top-level application),而不是在deps使用依赖项，这并不能完美地解决由于OTP文件结构引起的冲突.开发者之所以这样做，是因为他们想从源代码构建产品并使用自定义的命令来驱动(boot)他们的application.<br>
[注5]: http://learnyousomeerlang.com/release-is-the-word <br>
[注6]：https://github.com/erlware/relx/wiki]。
</font>
