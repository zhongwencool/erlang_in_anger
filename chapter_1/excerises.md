# Excerises
## Review Questions

[1]. How do you know if a code base is an application? A release?<br>
[2]. What differentiates an application from a library application?<br>
[3]. What can be said of processes under a one_for_all scheme for supervision?<br>
[4]. Why would someone use a gen_fsm behaviour over a gen_server?<br>
<font color="green">
[1]. 如何判断代码库是一个application，还是release?<br>
[2]. application和library application有什么区别？<br>
[3]. 什么场景会使用one_for_all监控模式?<br>
[4]. 什么场景会使用gen_fsm替换gen_server?<br>
</font>

## Hands-On

Download the code at https://github.com/ferd/recon_demo. This will be used as a test bed for exercises throughout the book. Given you are not familiar with the code base yet, let’s see if you can use the tips and tricks mentioned in this chapter to get an understanding of it.

[1]. Is this application meant to be used as a library? A standalone system?<br>
[2]. What does it do?<br>
[3]. Does it have any dependencies? What are they?<br>
[4]. The app’s README mentions being non-deterministic. Can you prove if this is true?
How?<br>
[5]. Can you express the dependency chain of applications in there? Generate a diagram
of them?<br>
[6]. Can you add more processes to the main application than those described in the
README?<br>
<font color="green">
下载代码：https://github.com/ferd/recon_demo. 这个是本书练习的实践代码，现在给你的是不熟悉的代码，实践下本章中提到的知识吧！<br>

[1]. 这个applicaiton是一个library?还是一个独立的系统(standalone system)? <br>
[2]. 它是用来做什么的？<br>
[3]. 他有依赖的application么？如果有，请列出来？<br>
[4]. README中提到的不确定的点，请验证是不是真的？<br>
[5]. 请描述一下applications的依赖关系链？生成一个图表？<br>
[6]. 请增加更多的进程(在README中提到的)到主applicaiton里面。<br>
</font>


