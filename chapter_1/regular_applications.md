# Regular Applications
For a regular OTP application, there are two potential modules that act as the entry point:
<p></p>
<font color="green" >
&emsp;对于一个regular OTP application，有2个可能的模块作为入口：
</font>
<p></p>
&emsp;1. appname </br>
&emsp;2. appname _app

<p></p>
The first file should be similar in use to what we had in a library application (an entry point), while the second one will implement the application behaviour, and will represent the top of the application’s process hierarchy. In some cases the first file will play both roles at once.
<p></p>
<font color="green" >
&emsp;appname用法和library application中的相似，而appname_app文件会实现application 的具体行为(behaviour)，它处于Applicaiton进程结构中的最顶层，在某些情况下，appname文件同时担当这两个角色。
</font>
<p></p>
If you plan on simply adding the application as a dependency to your own app, then look inside appname for details and information. If you need to maintain and/or fix the application, go for appname _app instead.
<p></p>
<font color="green" >
&emsp;如果你只是打算简单地将应用程序作为依赖项添加自已的app里面,你只需要看懂appname里面的信息和细节;
如果你需要操作或修复这个Application的问题，你就应该去看appname_app。
</font>
<p></p>
The application will start a top-level supervisor and return its pid. This top-level supervisor will then contain the specifications of all the child processes it will start on its own <sup>3</sup>.
The higher a process resides in the tree, the more likely it is to be vital to the survival of the application.
<p></p>
<font color="green" >
&emsp;Application 会启动最高层的监控进程(supervisor)并返回他的pid. 这个监控进程会含用自己启动的所有子进程的详细信息<sup>3</sup>。<br>
&emsp;进程所处树的位置越高，对application的生存影响越大。
</font>
<p></p>
You can also estimate how important a process is by the order it is started (all children in the supervision tree are started in order, depth-first). If a process is started later in the supervision tree, it probably depends on processes that were started earlier.
<p></p>
<font color="green" >
&emsp;你也要通过进程的重要性来决定进程启动顺序(所有的在supervisor下的子进程都会顺序启动)。如果进程E启动在进程[ABCD...]后启动，E就很有可能在启动时要依赖于[ABCD...]中的某些进程。
</font>
<p></p>
Moreover, worker processes that depend on each other within the same application (say, a process that buffers socket communications and relays them to a finite-state machine in charge of understanding the protocol) are likely to be regrouped under the same supervisor and to fail together when something goes wrong. This is a deliberate choice, as it is usually simpler to start from a blank slate, restarting both processes, rather than trying to figure out how to recuperate when one or the other loses or corrupts its state.
<p></p>
<font color="green" >
&emsp;此外，在同一个application中相互依赖的工作进程(例如：一个缓存socket信息并把socket交给一个负责解析协议的有效状态机的进程)会被分给同一个监控进程，这样可以其中一个出错后，2个进程同时失败(fail)掉。这是一个非常好的做法，因为重启它们就是小菜一碟，重启2个进程比试图恢复其中的错误进程简单得多。
</font>
<p></p>
The supervisor restart strategy reflects the relationship between processes under a supervisor:<br>
• **one_for_one** and **simple_one_for_one** are used for processes that are not dependent upon each other directly, although their failures will collectively be counted towards total application shutdown <sup>4</sup>.<br>
• **rest_for_one** will be used to represent processes that depend on each other in a linear manner.<br>
• **one_for_all** is used for processes that entirely depend on each other.<br>
This structure means it is easiest to navigate OTP applications in a top-down manner by exploring supervision subtrees.<br>
<p></p>
<font color="green" >
监控进程的重启策略由子进程间的相互关系决定：<br>
•** one_for_one** 和 **simple_one_for_one** <br>
&emsp;用于：子进程间没有直接相互依赖关系，但是他们失败会集中记录到application关闭<sup>4</sup>。<br>
• **rest_for_one** 会用于进程间有线性关系(linear manner)的场合。<br>
• **one_for_all** 用于一个进程异常，其它全部也会影响的场合。<br>
从这些结构可以看出：通过监控树(supervisior)的方式自上而下地驾驭OTP application是一件非常容易的事。
</font>
<p></p>

For each worker process supervised, the behaviour it implements will give a good clue about its purpose:<br>
• a **gen_server** holds resources and tends to follow client/server patterns (or more generally, request/response patterns)<br>
• a **gen_fsm** will deal with a sequence of events or inputs and react depending on them, as a Finite State Machine. It will often be used to implement protocols.<br>
• a **gen_event** will act as an event hub for callbacks, or as a way to deal with notifications of some sort.<br>
All of these modules will contain the same kind of structure: exported functions that represent the user-facing interface, exported functions for the callback module, and private functions, usually in that order.<br>
<font color="green" >
每一个工作进程都应当被监控，工作进程的behaviour会很好地表明其真正的目的：<br>
• **gen_server** 储存资源(holds resources)，倾向于遵循客户/服务器模式(client/server patterns),或更确切地说是，请求/应答模式(request/response patterns).<br>
•  **gen_fsm** 会处理一系列的事件或输入，或输入带来的一系列反应，就像一个有限状态机(Finite State Machine).常用于实现协议(implement protocols).<br>
• **gen_event** 作为一个事件回调中心，或用来处理某些形式的通知。<br>
&emsp;以上所有的模块都有着相同的结构：导出面向使用者的接口，导出回调接口，私有函数。通常在模块中的顺序也是如此。
</font>
<p></p>
Based on their supervision relationship and the typical role of each behaviour, looking at the interface to be used by other modules and the behaviours implemented should reveal a lot of information about the program you’re diving into.
<p></p>
<font color="green" >
&emsp;基于他们的监控关系和典型的behaviour角色，其它模块调用这些接口就可以给你提供很多信息，帮助你深入理解代码。
</font>
<p></p>
[3] In some cases, the supervisor specifies no children: they will either be started dynamically by some function of the API or in a start phase of the application, or the supervisor is only there to allow OTP environment variables (in the env tuple of the app file) to be loaded.<br>
[4] Some developers will use one_for_one supervisors when rest_for_one is more appropriate. They
require strict ordering to boot correctly, but forget about said order when restarting or if a predecessor
dies.
<p></p>
<font color="green" >
[注3]：在某些情况下，监控进程不会有这些详细信息，他们会被用API动态的创建出来;或者从OTP的环境变量中获得app file加载。<br>
[注4]：有些开发者在使用one_for_one的策略，但是实际上rest_for_one更合适。他们在进程死亡或重启时都需要遵守一定严格顺序。
</font>
