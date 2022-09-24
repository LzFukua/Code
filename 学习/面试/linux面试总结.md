###linux常用命令
**tail -f**filename 查看文件内容

**df** 查看文件系统磁盘使用情况

**ps -ef | grep ……** 显示当前进程的状态

**cat filename| head -n 300 | tail -n +100** 查看日志文件的100行到300行

**find** 在指定目录下查找文件

**su** 切换用户

**netstat -tunpl |grep端口号**   Linux查看端口是否占用命令

**top**   Linux查看CPU使用情况命令

**free**  Linux系统查看内存使用情况

**ps aux**查看进程



###Linux中 #! 表示什么意思？
用于指定由哪个解释器来执行脚本


###Linux中bash什么意思
bash就是增强版的shell语言

###shell脚本如何使用hive进行算术运算
sql = "..." 
hive -e "$sql" 


###shell脚本如何将标准输出和错误输出指定到同一位置 
 2>&1

###如何让shell脚本变成可执行文件
让脚本文件后缀名为.sh
执行Bash脚本或者是 sh 

###linux awk命令统计文件行数 普通查看行数的命令     
awk ‘END{print NR}’ filename 
wc -l
