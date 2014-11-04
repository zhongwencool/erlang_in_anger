# Project Structure

The structures of OTP applications and of OTP releases are different. An OTP application can be expected to have one top-level supervisor (if any) and possibly a bunch of dependencies that sit below it. An OTP release will usually be composed of multiple OTP applications, which may or may not depend on each other. This will lead to two major ways to lay out applications.
<p></p>
<font color="green">
&emsp;OTP applications和OTP release项目结构是不同的，一个OTP application 可以看成一个拥有最高级监控树(如果有的话)，并且下面可能有一大堆的依赖项(a bunch of dependencies).一个OTP release通常是多个OTP applications的组合，这些application之间可能会有依赖关系，也可能没有。这就形成了两种主要的部署applicaitons的方式。
<font>
<p></p>


