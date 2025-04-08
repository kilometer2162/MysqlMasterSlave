# mysql 主从同步

查了许多资料，最终发现一篇文章比较用心： https://zhaojun.vip/archives/97/

从库增加了 read_only 限制了只读，需要注意该选项只能限制平台用户，限制不了 root 权限用户。为了避免误操作，建议启动通过 set global super_read_only=true;(重启 MySQL 后失效)，这样会限制 root 权限用户也无法进行写入操作。（之所以没有把这个选项配置到配置文件中将限制无法进行连接）
集群中不同服务的 server_id 应避免重复。


如果是在windows上操作，两个my.cnf要将属性设置成只读即可。

一、请注意：

1、CHANGE MASTER TO 时要设置的 ​MASTER_LOG_FILE 与 MASTER_LOG_POS ，要与主库查询的 SHOW MASTER STATUS 一致，否则从库同步时会报Could not find first log file name in binary log index file异常。

2、MASTER_PORT 要改为数字否则执行不成功。



二、用到的一些命令:

    show master status;             #查看主库同步参数

    flush logs;                     #用于主库清空二进制日志并创建一个新的二进制日志文件    
    
    show slave status;              #查看从库同步状态
    
    stop slave;                     #停止同步
