MySQL 主从复制，不停机添加新从节点

1、主库创建账号：

```
show master status;
GRANT REPLICATION SLAVE ON . to 'reader'@'%' identified by 'readerpwd';
flush privilegs
```

2、从库配置

开启binlog

```+1
log-bin=/var/lib/mysql/mysql-bin
server-id=3	//参照原从库配置+1
```

3、备份主库

```
mysqldump -uroot -p123 --routines --single_transaction --master-data=2 --databases testdb > testdb.sql
```

参数说明：

- --routines：导出存储过程和函数
- --single_transaction：导出开始时设置事务隔离状态，并使用一致性快照开始事务，然后unlock tables;而lock-tables是锁住一张表不能写操作，直到dump完毕。
- --master-data：默认等于1，将dump起始（change master to）binlog点和pos值写到结果中，等于2是将change master to写到结果中并注释。

4、从库创建数据库，并导入数据

将dump的数据拷贝到从库后开始导数据

```
mysql> grant all pricileges on *.* to testdb.* identified by 'testdb';
mysql> create database testdb;
mysql> source /tmp/testdb.sql
```

5、查看备份文件的binlog 和 pos值

```
# head -25 testdb.sql
root@mysql20151:/tmp# head -25 /tmp/0907.sql 
-- MySQL dump 10.13  Distrib 5.5.46, for debian-linux-gnu (x86_64)
--
-- Host: localhost    Database: vphotos
-- ------------------------------------------------------
-- Server version       5.5.46-0ubuntu0.14.04.2-log

/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8 */;
/*!40103 SET @OLD_TIME_ZONE=@@TIME_ZONE */;
/*!40103 SET TIME_ZONE='+00:00' */;
/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;
/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;
/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;
/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;

--
-- Position to start replication or point-in-time recovery from
--

-- CHANGE MASTER TO MASTER_LOG_FILE='mysql-bin.003789', MASTER_LOG_POS=49778941;
```

> 可以看到 MASTER_LOG_FILE='mysql-bin.003789', MASTER_LOG_POS=49778941;

6、启动从库

```
mysql> change master to master_host='10.*.*.*',master_user='slave02',master_password='slave02@123',master_log_file='mysql-bin.003789',master_log_pos=49778941;
// 验证从库状态
mysql> show slave status\G;
*************************** 1. row ***************************
               Slave_IO_State: Waiting for master to send event
                  Master_Host: 10.3.16.7
                  Master_User: slave02
                  Master_Port: 3306
                Connect_Retry: 60
              Master_Log_File: mysql-bin.003791
          Read_Master_Log_Pos: 99002276
               Relay_Log_File: mysqld-relay-bin.000002
                Relay_Log_Pos: 253
        Relay_Master_Log_File: mysql-bin.003789
             Slave_IO_Running: Yes
            Slave_SQL_Running: Yes
              Replicate_Do_DB: 
          Replicate_Ignore_DB: 
          ..................
```

> 注:看到IO和SQL线程均为YES，说明主从配置成功。



参考：

https://yq.aliyun.com/articles/38826