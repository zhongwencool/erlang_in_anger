# Exercises
## Review Questions
## 复习问题：
[1]. Are Erlang supervision trees started depth-first? breadth-first? Synchronously or asynchronously?<br>
[2]. What are the three application strategies? What do they do?<br>
[3]. What are the main differences between the directory structure of an app and a release?<br>
[4]. When should you use a release?<br>
[5]. Give two examples of the type of state that can go in a process’ init function, and two examples of the type of state that shouldn’t go in a process’ init function<br>

<p></p> <font color="green">
[1]. Erlang supervision树启动时是深度优先(depth-first)或宽度优先(breadth-first)?<br> 同步地还是异步地？<br>
[2]. 三大application 策略是什么，他们是怎样做的？<br>
[3]. application和release文件夹结构的主要区别是什么？<br>
[4]. 什么情况下要使用release?<br>
[5]. 给出2个应该在init里面进行的例子？再给2个不应该在init里面进行的例子？<br>
</font> <p></p>

## Hands-On
## 亲手实践
Using the code at https://github.com/ferd/recon_demo:<br>
[1]. Extract the main application hosted in the release to make it independent, and includable in other projects.<br>
[2]. Host the application somewhere (Github, Bitbucket, local server), and build a release with that application as a dependency.<br>
[3]. The main application’s workers (council_member) starts a server and connects to it in its init/1 function. Can you make this connection happen outside of the init function’s? Is there a benefit to doing so in this specific case?
<p></p> <font color="green">

使用这里的code: https://github.com/ferd/recon_demo:<br>
[1]. 提取托管在release中的主application,让他变得独立，可被其它项目包含.<br>
[2]. 把application托管在(github,bibucket,locl server)上，然后使用application的依赖项来构建一个release.<br>
[3]. 主application的工作进程(council_member)开启了一个server并在init/1中连接到了它，请将这个连接搬到init/1外面来，再思考一下这一操作有什么具体的好处？
</font> <p></p>
