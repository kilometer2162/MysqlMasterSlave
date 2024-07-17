# mysql 主从同步

查了许多资料，最终发现一篇文章比较用心： https://zhaojun.vip/archives/97/

如果是在windows上操作，两个my.cnf要将属性设置成只读即可。

在主库执行命令查看主库的 binlog 当前文件名和位置：

mysql> SHOW MASTER STATUS;

返回：File: mysql-bin.000003     Position: 879 

在从库上配置从主库同步使用的信息：

CHANGE MASTER TO

    MASTER_HOST = 'mysql-master',        
    
    MASTER_PORT = '3306',
    
    MASTER_USER = 'repl',
    
    MASTER_PASSWORD = '123456',
    
    MASTER_LOG_FILE = 'mysql-bin.000003',
    
    MASTER_LOG_POS = 879;

请注意：此处​MASTER_LOG_FILE与MASTER_LOG_POS，要与 SHOW MASTER STATUS 一致，否则从库同步时会报Could not find first log file name in binary log index file异常。


用到的一些命令:

show master status;             #查看主库同步参数

flush logs;                     #用于主库清空二进制日志并创建一个新的二进制日志文件

show slave status;          #查看从库同步状态

stop slave;                 #停止同步

