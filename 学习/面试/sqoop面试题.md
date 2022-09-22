sqoop增量导入的方法  √
1. 用where 去过滤
2. 用columns 去导自己想要的字段
3. 用query 去写sql语句 后面需要加 AND $CONDITIONS  
一般用 query 去筛选时间和增量字段更多



--split-by og.rec_id \  
切分mr的优化

--hive-partition-key 'report_date' \ 
对这个字段分区


### sqoop 默认几个map
4个