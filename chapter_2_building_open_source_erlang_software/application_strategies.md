# Application Strategies
No matter what, a sequence of failures is not a death sentence for the node. Once a system has been divided into various OTP applications, it becomes possible to choose which applications are vital or not to the node.<br>
Each OTP application can be started in 3 ways: temporary, transient, permanent, either by doing it manually in application:start(Name, Type) , or in the config file for your release:
<p></p> <font color="green">
&emsp;不管怎样，一连串的失败对节点来说并不可怕。一旦系统被分成多个OTP applications时，就可以在节点上按重要性排序启动applications.<br>
&emsp;不论是手动用application:start(Name,Type)还是根据release里面的config文件启动，每一个OTP application 都可以有3种启动方式：暂时(temporary),短暂(transient),永久(permanent),
</font> <p></p>

• permanent: if the app terminates, the entire system is taken down, excluding manual termination of the app with application:stop/1.<br>
• transient: if the app terminates for reason normal, that’s ok. Any other reason for termination shuts down the entire system.<br>
• temporary: the application is allowed to stop for any reason. It will be reported, but nothing bad will happen.<br>
It is also possible to start an application as an included application, which starts it under your own OTP supervisor with its own strategy to restart it.
<p></p> <font color="green">
- 永久(permanent)：如果这个app被终结掉（除了手动调用application:stop/1终结的其它情况），整个系统都会挂掉。<br>
- 短暂(transient)：如果这个app 被正常理由(除了会把整个系统搞崩溃的原因外)终结的，这是ok的。<br>
- 暂时(temporary)：app允许随意停止，它只会报告状态，但不会引起什么错误事件。<br>
&emsp;也可以在一个application A中再启动另一个application B.让这个application B被application A中的 OTP supervisor根据相应的策略来操作重启B。
</font> <p></p>

