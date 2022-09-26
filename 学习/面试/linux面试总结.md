
### filename 查看文件内容(查看错误日志)
tail -f


### 查看文件系统磁盘使用情况
df

### 显示当前进程的状态
ps -ef | grep …… 

### 查看日志文件的100行到300行
cat filename| head -n 300 | tail -n +100

### 在指定目录下查找文件
find 

### 切换用户
su


### Linux查看端口是否占用命令
netstat -tunpl |grep端口号


### Linux查看CPU使用情况命令
top

### Linux系统查看内存使用情况
free

### 查看进程
ps aux

### Linux中 #! 表示什么意思？
用于指定由哪个解释器来执行脚本

## #Linux中bash什么意思
bash就是增强版的shell语言

### shell脚本如何使用hive进行算术运算
sql = "..." 
hive -e "$sql" 

### shell脚本如何将标准输出和错误输出指定到同一位置 
2>&1

### linux awk命令统计文件行数 普通查看行数的命令     
awk ‘END{print NR}’ filename 
wc -l


### shell脚本判断上一个命令是否执行成功
$? 如果返回0代表成功 其他代表失败

