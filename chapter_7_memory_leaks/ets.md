# ETS
ETS tables are never garbage collected, and will maintain their memory usage as long as
records will be left undeleted in a table. Only removing records manually (or deleting the
table) will reclaim memory.
In the rare cases you’re actually leaking ETS data, call the undocumented ets:i()
function in the shell. It will print out information regarding number of entries (size) and
how much memory they take (mem). Figure out if anything is bad.
It’s entirely possible all the data there is legit, and you’re facing the difficult problem
of needing to shard your data set and distribute it over many nodes. This is out of scope
for this book, so best of luck to you. You can look into compression of your tables if you
need to buy time, however. <sup>6</sup>

[6]See the compressed option for ets:new/2<br>
