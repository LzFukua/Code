###exists 和 in 的区别 
in 是把外表和内表作hash 连接，而exists是对外表作loop循环，每次loop循环再对内表进行查询。
如果查询的两个表大小相当，那么用in和exists差别不大。
**如果两个表中一个较小，一个是大表，则子查询表大的用exists，子查询表小的用in**

---

### innoDB 和 myisam的区别
1. InnoDB支持事务 , myisam不支持
2. InnoDB支持外键,myisam不支持
3. InnoDB是聚簇索引,myisam是非聚簇索引
4. 如果表中绝大多数都是读查询,就可以考虑myisam