http://www.vldb.org/pvldb/vol8/p1816-teller.pdf

* 跟之前在noah做的查询的优化思路类似。
* 20台机器1个T的内存用来在内存中构建tsdb
* 压缩算法，减少了90+%的空间占用，一个timeseries是存在一个shard上的
* correlation的检测很有意思
* 支持持久化
* 支持高可用

需要找时间读源码
