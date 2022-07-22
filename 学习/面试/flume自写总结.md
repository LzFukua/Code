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