# OTP Applications

For OTP applications, the proper structure is pretty much the same as what was explained in 1.2:
<font color="green">
&emsp;对于OTP applicaitons，合适的结构基本同1.2中描述的那样：<br>
----------------------------------------------------------------------------------
</font>
<p></p>
1 `doc/`<br>
2 `deps/`<br>
3 `ebin/`<br>
4 `src/`<br>
5 `test/`<br>
6 `LICENSE.txt`<br>
7 `README.md`<br>
8 `rebar.config`
<p></p>
----------------------------------------------------------------------------------<br>

What’s new in this one is the** deps/** directory, which is fairly useful to have, but that will be generated automatically by rebar <sup>2</sup> if necessary.<br>
That’s because there is no canonical package management in Erlang.  People instead adopted rebar, which fetches dependencies locally, on a per-project basis.<br>
This is fine and removes a truckload of conflicts, but means that each project you have may have to download its own set of dependencies.<br>
This is accomplished with rebar by adding a few config lines to rebar.config:
<p></p>
<font color="green">
&emsp;新加的文件夹:**deps/** 非常有用的一个文件夹。这个文件夹如果有必要也可以直接通过rebar自动生成<sup>2</sup>。<br>
这是因为在Erlang没有标准的包管理器(canonical package management)。现在的做法是用rebar来做为每一个项目在本地获取依赖项的工作。<br>
&emsp;这可以解决一大堆的冲突，但也意味着生个项目都要下载自己的依赖项目啦。<br>
&emsp;下面是通过在rebar.config里面增加一些配置项来配置rebar：<br>
</font>
----------------------------------------------------------------------------------<br>
1 `{deps,`<br>
2 `[{application_name, "1.0.*",`<br>
3 `{git, "git://github.com/user/myapp.git", {branch,"master"}}},`<br>
4 `{application_name, "2.0.1",`<br>
5 `{git, "git://github.com/user/hisapp.git", {tag,"2.0.1"}}},`<br>
6 `{application_name, "",`<br>
7 `{git, "https://bitbucket.org/user/herapp.git", "7cd0aef4cd65"}},`<br>
8 `{application_name, "my regex",`<br>
9 `{hg, "https://bitbucket.org/user/theirapp.hg" {branch, "stable"}}}]}.`<br>

----------------------------------------------------------------------------------<br>
<p></p>
Feel free to install rebar globally on your system, or keep a local copy if you require a specific version to build your system.
Applications are fetched directly from a git (or hg, or svn) source, recursively. They can then be compiled, and specific compile options can be added with the {erl_opts, List}.option in the config file <sup>3 </sup>.<br>
Within these directories, you can do your regular development of an OTP application.<br>
To compile them, call rebar get-deps compile, which will download all dependencies,and then build them and your app at once.<br>
When making your application public to the world, distribute it without the dependencies. It’s quite possible that other developers’ applications depend on the same applications yours do, and it’s no use shipping them all multiple times.<br>
The build system in place (in this case, rebar) should be able to figure out duplicated entries and fetch everything necessary only once.
<p></p>
 <font color="green">
&emsp;rebar会递归地从git(或hg,或svn)中直接拿到application的源代码。他们可以被编译，并使用特定的选项{erl_opts,List}来编译. 这个选项也在config文件中定义<sup>3 </sup>。<br>
你可以在这些文件夹中，为自己的OTP application开发功能。<br>
&emsp;为了编译他们，你可以使用rebar get-deps把依赖项也下载下来编译，并马上构建依赖项和你自己的app。<br>
&emsp;当你把自己的application开源出来时，要把它的依赖项给去掉。因为其他开发者的application很有可能和你一样依赖同样的application，没有必要重复装载他们多次。<br>
&emsp;构建系统(在这里，指rebar）应当能判断出重复的依赖项并这些重复的项只会被加载一次。
</font>
<p></p>
[2] A lot of people package rebar directly in their application. This was initially done to help people who had never used rebar before use libraries and projects in a boostrapped manner.<br>
[3] More details by calling rebar help compile.
 <p></p>
 <font color="green">
[注2]：很多人都是把rebar集成在自己的applicaiton中。这最初是为了帮助那些从来不使用rebar的人快速掌握这个工具，你可以在系统里全局自由安装rebar，也可以在局部保存一个特定的版本来构建你的系统。<br>
[注3]：你可以输入rebar help来查看更多的帮助信息。<br>
 </font>

