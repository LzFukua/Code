sqoop增量导入的方法  √
1. 用where 去过滤
2. 用columns 去导自己想要的字段
3. 用query 去写sql语句 后面需要加 AND $CONDITIONS  
一般用 query 去筛选时间和增量字段更多