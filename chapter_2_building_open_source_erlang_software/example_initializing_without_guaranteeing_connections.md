#  Example: Initializing without guaranteeing connections
## 示例：没有保证连接的初始化
The following code attempts to guarantee a connection as part of the process’ state:
<p></p> <font color="green">

下面代码认为进程可以在初始化过程中必然连接成功。
</font> <p></p>

----------------------------------------------------------------------------------<br>
1 `init(Args) ->`<br>
2 `Opts = parse_args(Args),`<br>
3 `{ok, Port} = connect(Opts),`<br>
4 `{ok, #state{sock=Port, opts=Opts}}.`<br>
5 <br>
6 `[...]`<br>
7<br>
8 `handle_info(reconnect, S = #state{sock=undefined, opts=Opts}) ->`<br>
9 `%% try reconnecting in a loop`<br>
10 `case connect(Opts) of`<br>
11 `          {ok, New} -> {noreply, S#state{sock=New}};`<br>
12 `          _ -> self() ! reconnect, {noreply, S}`<br>
13 `end;`<br>
----------------------------------------------------------------------------------<br>
Instead, consider rewriting it as:
<p></p> <font color="green">
重写如下：
</font> <p></p>

----------------------------------------------------------------------------------<br>
1 `init(Args) ->`<br>
2 `Opts = parse_args(Args),`<br>
3 `%% you could try connecting here anyway, for a best`<br>
4` %% effort thing, but be ready to not have a connection.`<br>
5 `self() ! reconnect,`<br>
6` {ok, #state{sock=undefined, opts=Opts}}.`<br>
7  <br>
8 `[...]`<br>
9 <br>
10` handle_info(reconnect, S = #state{sock=undefined, opts=Opts}) ->`<br>
11` %% try reconnecting in a loop`<br>
12` case connect(Opts) of`<br>
13` {ok, New} -> {noreply, S#state{sock=New}};`<br>
14` _ -> self() ! reconnect, {noreply, S}`<br>
15` end;`<br>
----------------------------------------------------------------------------------<br>
You now allow initializations with fewer guarantees: they went from the connection is available to the connection manager is available.
<p></p> <font color="green">

重写后，能保证初始化必然成功，因为他的重连都是在初始化之后的handle_info中。
</font> <p></p>
