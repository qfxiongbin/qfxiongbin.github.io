# MySQL常见问题整理


<!-- {{< version 0.2.0 >}} -->

<!--more-->

## 无法重启

> 报错信息
```
2022-11-13T11:27:13.455198Z 0 [Warning] [MY-000081] [Server] option 'max_allowed_packet': unsigned value 107374182400 adjusted to 1073741824.
2022-11-13T11:27:13.455277Z 0 [Warning] [MY-010915] [Server] 'NO_ZERO_DATE', 'NO_ZERO_IN_DATE' and 'ERROR_FOR_DIVISION_BY_ZERO' sql modes should be used with strict mode. They will be merged with strict mode in a future release.
2022-11-13T11:27:13.455373Z 0 [System] [MY-010116] [Server] /www/server/mysql/bin/mysqld (mysqld 8.0.24) starting as process 27708
2022-11-13T11:27:13.518045Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
2022-11-13T11:28:39.973579Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.

InnoDB: Progress in percents: 12022-11-13T11:28:40.484074Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Bind-address: '::' port: 33060, socket: /tmp/mysqlx.sock
2022-11-13T11:28:40.837360Z 0 [System] [MY-010229] [Server] Starting XA crash recovery...
2022-11-13T11:28:40.850611Z 0 [System] [MY-010232] [Server] XA crash recovery finished.
2022-11-13T11:28:41.101314Z 0 [Warning] [MY-010068] [Server] CA certificate ca.pem is self signed.
2022-11-13T11:28:41.101688Z 0 [System] [MY-013602] [Server] Channel mysql_main configured to support TLS. Encrypted connections are now supported for this channel.
2022-11-13T11:28:41.127441Z 0 [System] [MY-010931] [Server] 

[root@ecs-733a home]# ps -ef|grep mysql
root     11456     1  0 1月19 ?       00:00:00 /bin/sh /www/server/mysql/bin/mysqld_safe --datadir=/www/server/data --pid-file=/www/server/data/ecs-733a.pid
root     26896 19969  0 19:26 pts/0    00:00:00 grep --color=auto mysql
mysql    27114 11456 40 7月20 ?       47-06:58:41 /www/server/mysql/bin/mysqld --basedir=/www/server/mysql --datadir=/www/server/data --plugin-dir=/www/server/mysql/lib/plugin --user=mysql --log-error=ecs-733a.err --open-files-limit=65535 --pid-file=/www/server/data/ecs-733a.pid --socket=/tmp/mysql.sock --port=63306
```

> 分析

刚开始以为是数据量太大无法启动，其中一张历史表数据两千万左右的采集数据,实际上不是

> 解决

将 11456 与 27114 kill
重启，正常启动

## 更新大批量数据 提示Lock wait timeout exceeded; try restarting transaction

等待时间超时导致。

```
 
 有时候看看show engine innodb status , 并结合 show full processlist;能暂时解决问题；但一直不能精确定位；

在5.5中，information_schema库中增加了三个关于锁的表（MEMORY引擎）；

innodb_trx ## 当前运行的所有事务

innodb_locks ## 当前出现的锁

innodb_lock_waits ## 锁等待的对应关系

数据库配置文件修改：
原因：原因是你使用的InnoDB  表类型的时候,

默认参数:innodb_lock_wait_timeout设置锁等待的时间是50s,

因为有的锁等待超过了这个时间,所以抱错.

你可以把这个时间加长,或者优化存储过程,事务避免过长时间的等待.

解决的办法有两个：

第一：innodb_lock_wait_timeout 锁定等待时间改大

my.ini文件：

#innodb_lock_wait_timeout = 50

修改为

innodb_lock_wait_timeout = 500
```
