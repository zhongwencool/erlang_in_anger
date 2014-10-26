# Chapter 9 Tracing
One of the lesser known and absolutely under-used features of Erlang and the BEAM virtual
machine is just ab out how much tracing you can do on there.
<br>&emsp;Forget your debuggers, their use is to o limited. <sup>1</sup> Tracing makes sense in Erlang at all
steps of your system’s life cycle, whether it’s for development or for diagnosing a running
production system.
<br>&emsp;There are a few options available to trace Erlang programs:
<br>&emsp;• **sys** <sup>2</sup> comes standard with OTP and allows to set custom tracing functions, log all
kinds of events, and so on. It’s generally complete and fine to use for development. It
suffers a bit in production because it doesn’t redirect IO to remote shells, and doesn’t
have rate-limiting capabilities for trace messages. It is still recommended to read the
documentation for the module.
<br>&emsp;• **dbg** <sup>3</sup> also comes standard with Erlang/OTP. Its interface is a bit clunky in terms of
usability, but it’s entirely good enough to do what you need. The problem with it is
that you have to know what you’re doing, because dbg can log absolutely everything
on the node and kill one in under two seconds.
<br>&emsp;• *tracing **BIF**s* are available as part of the erlang module. They’re mostly the raw
blocks used by all the applications mentioned in this list, but their lower level of
abstraction makes them rather difficult to use.
<br>&emsp;• **redbug** <sup>4</sup> is a production-safe tracing library, part of the eper 5 suite. It has an internal
rate-limiter, and a nice usable interface. To use it, you must however be willing to
add in all of eper’s dependencies. The toolkit is fairly comprehensive and can be a
very interesting install.
<br>&emsp;• **recon_trace** <sup>6</sup> is recon’s take on tracing. The objective was to allow the same levels
of safety as with redbug, but without the dependencies. The interface is different,
and the rate-limiting options aren’t entirely identical. It can also only trace function
calls, and not messages. <sup>7</sup>
<br>&emsp;This chapter will focus on tracing with **recon_trace**, but the terminology and the
concepts used mostly carry over to any other Erlang tracing tool that can be used.

[1]One common issue with debuggers that let you insert break points and step through a program is that
they are incompatible with many Erlang programs: put a break point in one process and the ones around
keep going. In practice, this turns debugging into a very limited activity because as soon as a process needs
to interact with the one you’re debugging, its calls start timing out and crashing, possibly taking down the
entire node with it. Tracing, on the other hand, doesn’t interfere with program execution, but still gives
you all the data you need.<br>
[2] http://www.erlang.org/doc/man/sys.html<br>
[3] http://www.erlang.org/doc/man/dbg.html<br>
[4] https://github.com/massemanet/eper/blob/master/doc/redbug.txt<br>
[5] https://github.com/massemanet/eper<br>
[6] http://ferd.github.io/recon/recon_trace.html<br>
[7] Messages may be supported in future iterations of the library. In practice, the author hasn’t found
the need when using OTP, given behaviours and matching on specific arguments allows to get something
roughly equivalent.<br>
