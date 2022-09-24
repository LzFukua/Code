### 什么是脑裂
由于假死会发起新的Leader选举，选举出一个新的Leader，但旧的Leader网络又通了，导致出现了两个Leader ，有的客户端连接到老的Leader，而有的客户端则连接到新的leader。

### zookeeper如何解决脑裂的
过半机制,也就是当旧皇帝驾崩的时候,选新皇帝时必须要超过一半的人投票选举成功才可以让他当上皇帝,否则就不会让他当上.并且leader有年号epoch,如果大家看到他的年号落伍了就不理他.


### zookeeper在HDFS的故障转移机制
自动故障转移机制：
1. active Namenode宕机(假死)。
2. active Namenode zkfc检测到假死
3. 通知另一台namenode的zkfc
4. 另一台机机器强行杀死之前的active namenode
5. 激活standby namenode，切为active状态



###  zookeeper是什么
ZooKeeper是一个分布式应用程序协调服务，用作主从协调，服务器节点上下线，元数据管理


### zookeeper是怎么保证HDFS高可用的