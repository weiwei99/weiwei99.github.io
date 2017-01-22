https://raft.github.io/slides/uiuc2016.pdf 讲的很清楚

通过raft/doc.go看清楚了脉络

http://www.infoq.com/cn/articles/etcd-interpretation-application-scenario-implement-principle/

mvcc实现：
* 内存中B+树   key-> revisions
* 对应的backend是磁盘上的bolt，bolt里存的是revisions -> serilized(k,v),bolt本身是B+树实现的kv，支持range操作


EtcdServer) run是主循环

raftReadyHandler来解耦了上层存储逻辑和底层raft同步

针对bolt的特点，本身etcd并不能处理大的数据量
