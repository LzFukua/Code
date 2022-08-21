### Kafka在Zookeeper中存储的信息有哪些？
1. broker相关信息
2. 控制器所在broker的信息
3. 所有消费者的信息
4. 集群管理信息 (partition重分配信息,最优选举leader的副本信息,近期删除的topic信息)
5. ISR变更信息
