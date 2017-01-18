https://static.googleusercontent.com/media/research.google.com/en//pubs/archive/44686.pdf

广告系统整体上是一个流式系统，在构建multi-home的时候关键是所有时间消费且仅消费一次，并且如果部分集群不可用时，通过checkpoint要能够在满足一致性的条件下重新追回来

从single home加一系列的failover工具到multi-home，paxos是基础

F1 spanner

Photon

Mesa
