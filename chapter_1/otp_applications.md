# OTP Applications

Figuring out OTP applications is usually rather simple. They usually all share a directory structure that looks like:
<p></p>
<font color="green" >
&emsp;搞清楚 OTP applications通常都非常简单，他们通常在同一个目录下，
目标结构如下：
</font>
<p></p>
<p>
 `doc/;`<br>
 `ebin/`<br>
 `src/`<br>
 `test/`<br>
 `LICENSE.txt`<br>
 `README.md`<br>
 `rebar.config`<br>
</p>
There might be slight differences, but the general structure will be the same.
Each OTP application should contain an app file, either ebin/<AppName>.app or more often, src/<AppName>.app.src <sup>2</sup>. There are two main varieties of app files:
<p></p>
<font color="green" >
具体的项目可能会有轻微差异，但通用结构都是一样的。
每个OTP application 会包含一个app文件，放在 ebin/<AppName>.app 或更常见放在src/<AppName>.app.src <sup>2</sup>， 以下是2类app文件的结构：
</font>
<p></p>
 library application:<br>
`{application, useragent, [` <br>
`{description, "Identify browsers & OSes from useragent strings"},` <br>
`{vsn, "0.1.2"}, `<br>
`{registered, []},` <br>
`{applications, [kernel, stdlib]},` <br>
`{modules, [useragent]}` <br>
`]}.` <br>
 regular application：<br>

`{application, dispcount, [`<br>
`{description, "A dispatching library for resources and task "`<br>
`"limiting based on shared counters"},`<br>
`{applications, [kernel, stdlib]},`<br>
`{registered, []},`<br>
`{mod, {dispcount, []}},`<br>
`{modules, [dispcount, dispcount_serv, dispcount_sup,`<br>
`dispcount_supersup, dispcount_watcher, watchers_sup]}`<br>
`]}.`
<p></p>
 [2]  A build system generates the final file that goes in ebin. Note that in these cases, many
src/<AppName>.app.src files do not specify modules and let the build system take care of it.
<p></p>
<font color="green" >
 [注2]：最终会用构建系统生成.app文件放到ebin/下,而且大部分的src/<AppName>.app.src不会列模块列表，构建系统会自动加上去的。
 </font>
