influxdb：时序数据库，针对监控数据设计的数据库，提供良好的读写性能，重点研究了对应的存储

概念和模型：https://docs.influxdata.com/influxdb/v1.1/concepts/key_concepts/

* measurement： 例如cpu
* tag: 数据可以加上一系列的tag，例如 host=machine1,service=www
* series: measurement+tag
* fields: 例如cpu的field可以是简单的一个value=50，也可以是cpu_idle=10,cpu_user=20等
* key: series+field

存储结构：
* cache + wal + tmsl + meta
* cache是最新的数据的内存缓存
* wal是cache在磁盘上的存储
* wal太大之后就要snapshot到tmsl文件，tmsl生成之后就是只读的
* tmsl周期性compact来合并成更大的文件
* tmsl = header + block + index;其中index里指明了每个key对应的一组数据在block里的offset，以及对应时间的start和end，index本身是按照key来排序的
* 写数据的过程就是写cache和wal，因为wal完全是追加写，不存在随机写，所以写的效率可以保证
* 读数据需要从tmsl和cache里去读
* 为了可以更快的删除历史过期数据，引入了shard， 一个db分成了多个shard，一个shard存储一段连续时间的数据，一旦shard过期就整体删除
