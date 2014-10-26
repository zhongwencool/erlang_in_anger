# Global View
# 全局视图
For a view of the VM in the large, it’s useful to track statistics and metrics general to the VM, regardless of the code running on it. Moreover, you should aim for a solution that allows long-term views of each metric — some problems show up as a very long accumulation over weeks that couldn’t be detected over small time windows.<br>
&emsp;Good examples for issues exposed by a long-term view include memory or process leaks, but also could be regular or irregular spikes in activities relative to the time of the day or week, which can often require having months of data to be sure about it.
<p></p> <font color="green">
&emsp;对于大型的VM,跟踪统计(statistics)和指标(metrics)通常对VM很有用，不是针对运行时的代码。此外，你应该致力于允许长时间观察每个指标的方案---一些问题只能通过长时间的累积才浮现出来。<br>
&emsp;一个需要长时间观测才能暴露的典型例子：内存或进程泄漏，当然也可能是在一天或一周时间内相关活动的一些规则或不规则的峰值造成的，这可往往需要几个月的数据才能找出问题。
</font> <p></p>
&emsp;For these cases, using existing Erlang metrics applications is useful. Common options are:<br>
&emsp;• **folsom** <sup>3</sup> to store metrics in memory within the VM, whether global or app-specific..<br>
&emsp;• **vmstats**&emsp; <sup>4</sup> and statsderl <sup>5</sup>, sending node metrics over to graphite through statsd <sup>6</sup>.<br>
&emsp;• **exometer** <sup>7</sup>, a fancy-pants metrics system that can integrate with folsom (among other things), and a variety of back-ends (graphite, collectd, statsd, Riak, SNMP, etc.). It’s the newest player in town<br>
&emsp;• **ehmon** <sup>8</sup> for output done directly to standard output, to be grabbed later through  specific agents, splunk, and so on.<br>
&emsp;• **custom** hand-rolled solutions, generally using ETS tables and processes periodically  dumping the data. <sup>9</sup><br>
&emsp;• or if you have nothing and are in trouble, a function printing stuff in a loop in a shell <sup>10</sup>.<br>
It is generally a good idea to explore them a bit, pick one, and get a persistence layer that will let you look through your metrics over time.
<p></p> <font color="green">
对于这些情况，使用现有的Erlang度量application非常有用，常用的选择如下：<br>
&emsp;• **folsom**<sup>3</sup>把指标储存在VM的内存中，可以指定是全局的还是app所特有的。<br>
&emsp;• **vmstats**<sup>4</sup> 和 **statsderl**<sup>5</sup>使用statsd<sup>6</sup>发送节点的指标。<br>
&emsp;• **exometer**<sup>7</sup>：<br>
    &emsp;一个可以整合folsom(还有其它的)，各式各样的back-ends(graphite,collectd,statsd,Riak,SNMP等)的非常庞大的系统。<br>
&emsp;• **ehmon**<sup>8</sup>把输出直接放到标准输出上，可以被其它特定的代理所捕获。<br>
&emsp;• **自定义的方案**：通常是使用ETS表，进程定期的dumping数据<sup>9</sup>。<br>
&emsp;•  或许你根本就没有什么麻烦，你只需要一个函数在loop里面打印出东西到shell上就行了<sup>10</sup>。<br>
你可以都大概小浏览下他们，然后选一个，仔细研究下，让它可以随时看到你想了解的指标。
</font> <p></p>

<p></p>
[3] https://github.com/boundary/folsom<br>
[4] https://github.com/ferd/vmstats<br>
[5] https://github.com/lpgauth/statsderl<br>
[6] https://github.com/etsy/statsd/<br>
[7] https://github.com/Feuerlabs/exometer<br>
[8] https://github.com/heroku/ehmon<br>
[9] Common patterns may fit the ectr application, at https://github.com/heroku/ectr<br>
[10] The recon application has the function recon:node_stats_print/2 to do this if you’re in app
<p></p> <font color="green">
[注3]： https://github.com/boundary/folsom<br>
[注4]： https://github.com/ferd/vmstats<br>
[注5]： https://github.com/lpgauth/statsderl<br>
[注6]： https://github.com/etsy/statsd/<br>
[注7]： https://github.com/Feuerlabs/exometer<br>
[注8]： https://github.com/heroku/ehmon<br>
[注9]： Common patterns may fit the ectr application, at https://github.com/heroku/ectr<br>
[注10]：recon application 有一个函数可以在app里调用recon:node_stats_print/2来做这件事。
</font> <p></p>

