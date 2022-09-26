### 关于flume
核心角色是agent,相当于一个传输通道,里面有三个组件
sources 负责读取数据
channel 缓存读取出来的数据
sink  写出数据


### 自定义拦截器的开发步骤 , 完成需求
我所认为的拦截器就像一个交通指挥官,把不同种类的日志分发到不同的分析系统,其主要的点在更改flume的event的header的value值

自定义首先得导入依赖,然后创建一个自定义的拦截器类去实现Interceptor接口,重写里面的四个方法
1.初始化拦截器
2.处理每条事件
3.处理每批事件
4.结束拦截器
然后构建一个拦截器的构建器,因为自定义拦截器的类无法直接new，需要通过flume配置文件调用静态内部类，来间接地调用自定义的拦截器对象。
打一个有依赖的包传入到flume的lib文件下,配置interceptor type 为包名就可以实现了



### flume的事务了解吗? 说一下flume的事务!  事务分类 , 事务的流程 ,事务效果 !
了解的,首先flume事务他的就是为了保证数据的可靠传递嘛,他分成了两个数据传递的事务处理,一个是put事务,一个是take事务,我们首先来说说put事务
1. put事务是source端向channel端去传递数据的时候触发的,source会采集数据封装为event,调用doput方法将这批event写到putlist临时缓存区,写满了就进行docommit去检查channel中是否有足够空间去装这批数据,如果可以,putlist就会清空这批数据到channel里,但如果channels容量不足,就进行rollback回滚的操作,清空putlist,然后返回异常给sources,重新采集一遍这批数据进来.

2. 而take事务就是sink端从channel端拉取数据的时候触发的,首先事务开始时调用doTake方法将channel里的event剪切到takeList中,同时也会拷贝一份放到HDFS的IO缓冲流中,当takeList满了之后就会触发doCommit方法,如果数据全部发送成功就会去调用IO流的flush方法将数据写道HDFS磁盘上,同时清空takelist的数据,但如果失败了,就会进行回滚操作,让takelist里的数据重新放入到channel.但这时候可能会导致数据重复,是因为当IO缓存区有一些数据已经传输成功了但回滚操作是把整个takelist的数据都回滚回去重新拉取.

而事务有三个比较重要的参数 一个是sources和sink端的batchSize 以及channel端的capacity 和transactionCapacity  capacity > transactionCapacity > batchSize




### flume拦截器怎么写的
首先添加依赖,然后实现Interceptor接口,重写里面的四个方法,分别是初始化执行一次的方法,结束后执行一次的方法,每个事件的处理方式,每批事件的处理方式,然后打包放到flume的lib文件夹,修改配置文件,将自定义拦截器的类写在配置上

### Flume HDFS Sink 小文件处理
开启日志文件定时回滚,不要产生大小文件
根据时间5min(300)回滚文件
  a1.sinks.k1.hdfs.rollInterval= 300
当临时文件达到128M(134217728)时文件回滚,这是压缩前的数值
  a1.sinks.k1.hdfs.rollSize=134217728
不根据event数量回滚文件
  a1.sinks.k1.hdfs.rollCount= 0


### Flume 整套配置
第一级
1. source 用的是TailDir 会对文件夹监控,且有position偏移量文件来保证数据不会丢失以及断点续传的功能
2. channel 用的是Filechannel 缓存存储在磁盘上且有checkpoint副本保证数据的安全
3. sink 用的是avro

在第一级的每个agent上都配置两个sink,分别指向第二级的主备节点汇聚上 形成failover sink组
底层的原理就是优先级高的sink先工作 优先级高的宕机了 就切换节点 然后会有退避时间去询问主节点是否恢复

第二级
1. avro source
2. filechannel 
3. hdfs sink 配置 且配置了多sink 也就是选择器去分摊压力并行写数据 (也可以扯到小文件 )


### 数据零点漂移问题
我们开发了自定义拦截器去抽取日志数据的时间戳放入event的header,以便于sink可以根据时间戳来落盘,而且我们把计算时间放到了1点钟才开始,并且在外面写了脚本统计文件行数,如果少了很多就会重跑,少了一些就不跑但一定会在2点执行,且跑的时候后面的数据就不要了

### 数据会重复吗
会的,因为flume的事务机制能保证不丢失不能保证步重复(解决方法就是在入库时去重,开发了一个数据服务系统,一个可视化的界面台,我负责的内容就是开发日志上报服务和日志采集条数服务用到了一点spring boot mybatis,然后就可以用脚本写出判断是否需要去重,然后入库时候就会去跑去重任务)
