
### Flink的四大基石
1. Checkpoint  Checkpoint 本质就是对State的备份, 持久化到文件系统比如HDFS中
JobManagerCheckpointStorage  FileSystemCheckpointStorage
2. State 
旧版
MemoryStateBackend  基于内存存储
FsStateBackend  基于文件存储
RocksDBStateBackend  基于RocksDB数据库存储 

新版
HashMapStateBackend 等价于   MemoryStateBackend  FsStateBackend 多了个路径
EmbeddedRocksDBStateBackend  等价于RocksDBStateBackend
3. Time 时间语义

4. Window 将无限的数据流划分成多个有限数据流的手段 然后对每个小的有限数据流进行批次处理
事件时间的度量标志为水平线, 事件时间应用于窗口中


### watermark的意义
1. 标识Flink任务的时间时间进度，从而推动时间时间窗口的触发和计算
2. 解决时间时间窗口的乱序问题
flink1.11中对flink的水印生成接口进行了重构，创建watermark主要有以下三种方式
1）使用createWatermarkGenerator 创建watermark。
2）使用固定延时策略生成水印，调用WatermarkStrategy中的静态方法forBoundedOutOfOrderness。
3）使用单调递增的方式生成水印，调用WatermarkStrategy中的静态方法forMonotonousTimestamps

### Flink时间语义
Flink在1.12版本后默认使用Event Time
1. 处理时间（Process Time）系统时间
2. 事件时间（Event Time）数据源提供的时间
3. 摄取时间（Ingestion Time）数据进入flink的时间


### 如何保证数据不丢失
Flink不能保证数据不会丢失，最多只能通过设置较大的延迟时间来让乱序的事件到达。


### Flink保证Exactly Once语义
举例kafka保证Exactly Once
1. source阶段 把kafka消费者的消费位移记录在算子状态中
2. sink阶段
   1. 采用幂等写入方式
   2. 采用两阶段提交事务写入方式
   3. 采用预写日志2PC提交方式




### flink checkpoint的原理 以及怎么实现的
Flink分布式快照算法,异步 barrier 快照

### Flink容错机制
一个为checkpoint，一个为savepoint，checkpoint默认为自动保存到状态后端。

### 检查点分界线 barrrier
jobmanager会给每个任务做完的时候遇到了barrier就让他分身拍照，另一份数据会继续往下流。
### 分布式快照算法
多个上游任务向同一个下游传递任务的时候，就需要让所有的分界线都到齐了才能开始状态保存，这样可以保证数据有序，但flink好像是一点几版本的时候就可以设置不需要到齐也可以继续往下流的保存方式了，也被称为异步快照