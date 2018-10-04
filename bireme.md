



# Bireme 数据同步

## 介绍

1、Bireme 是一个 Greenplum / HashData 数据仓库的**增量**同步工具。目前支持 MySQL、PostgreSQL 和 MongoDB 数据源。

2、[Greenplum](http://greenplum.org/) 是一个高级，功能齐全的开源数据仓库，为PB级数据量提供强大而快速的分析。它独特地面向大数据分析，由世界上最先进的基于成本的查询优化器提供支持，可在大数据量上提供高分析查询性能。

 3、Bireme 采用 DELETE + COPY 的方式，将数据源的修改记录同步到 Greenplum / HashData ，相较于INSERT + UPDATE + DELETE的方式，COPY 方式速度更快，性能更优。

4、[HashData](http://www.hashdata.cn/) 则是基于 Greenplum 构建弹性的云端数据仓库。

Bireme 特性与约束：

- 采用小批量加载的方式提升数据同步的性能，默认加载延迟时间为10秒钟。
- 所有表在目标数据库中必须有主键

## 1. 数据流

![](/home/ljohn/文档/Markdown/pictures/data_flow.png)

## 2. 数据源

2.1 Maxwell + Kafka 是 bireme 目前支持的一种数据源类型，架构如下图：

![](/home/ljohn/文档/Markdown/pictures/maxwell.png)

- [Maxwell](http://maxwells-daemon.io/) 是一个 MySQL binlog 的读取工具，它可以实时读取 MySQL 的 binlog，并生成 JSON 格式的消息，作为生产者发送给 Kafka。

2.2 Debezium + Kafka 是 bireme 支持的另外一种数据源类型，架构如下图：

![](/home/ljohn/文档/Markdown/pictures/debezium.png)

## 3.Bireme 工作原理

Bireme 从数据源读取数据 (Record)，将其转化为内部格式 (Row) 并缓存，当缓存数据达到一定量，将这些数据合并为一个任务 (Task)，每个任务包含两个集合，delete 集合与insert 集合，最后把这些数据更新到目标数据库。

 每个数据源可以有多个 pipeline，对于 maxwell，每个 Kafka partition 对应一个 pipeline；对于 debezium，每个 Kafka topic 对应一个 pipeline



## 4. 配置MySQL 同步数据到GreenPlum

**这里bireme 使用Maxwell + Kafka 作为数据来源**

#### 4.1 MySQL 数据源

```
1、数据库，create database syncdb;
2、用户权限，需要拥有select权限和binlog拉取权限，此处使用root权限
3、同步的表（切换到syncdb1数据库），create table tb1(a int, b char(10), primary key(a));
```

#### 4.2 GP 目的数据库

```
1、用户，create user syncdb with password 'syncdb';
2、数据库，create database syncdb with owner 'syncdb';
3、同步的表（使用syncdb用户切换到syncdb数据库），create table tb1(a int, b char(10), primary key(a));
```

#### 4.3 环境准备

```
准备java 环境
sudo apt-get install openjdk-8-jdk 
sudo apt-get install jsvc
```



## 5. Kafka 配置

1、下载地址：<https://mirrors.tuna.tsinghua.edu.cn/apache/kafka/>
2、kafka官方文档：<http://kafka.apache.org/>
3、解压缩：tar xf kafka_2.11-2.0.0.tgz && cd kafka_2.11-2.0.0
4、ZooKeeper

```
启动，bin/zookeeper-server-start.sh config/zookeeper.properties
关闭，bin/zookeeper-server-stop.sh config/zookeeper.properties
```

5、Kafka server

```
启动，bin/kafka-server-start.sh config/server.properties
启动，bin/kafka-server-stop.sh config/server.properties
```

6、Topic

```
创建，bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic world
查询，bin/kafka-topics.sh --list --zookeeper localhost:2181
删除，bin/kafka-topics.sh --delete --zookeeper localhost:2181 --topic world
```

7、Producer（不是本实验必须的，作为学习使用）

```
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test
>hello
>ljohn
>
```

8、Consumer（不是本实验必须的，作为学习使用）

```
bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test --from-beginning
hello
ljohn
```

## 6.maxwell 配置

1、下载地址：<https://github.com/zendesk/maxwell/releases>
2、maxwell官方文档：<https://github.com/zendesk/maxwell>
3、解压缩：tar xf maxwell-1.17.1.tar.gz && cd maxwell-1.17.1
4、修改配置文件，cp config.properties.example config.properties && vim config.properties

```
log_level=info
client_id=2 

producer=kafka
kafka.bootstrap.servers=localhost:9092
kafka_topic=mytopic
ddl_kafka_topic=mytopic

# mysql login info
host=10.3.151.199
port=3306
user=root
password=root
replica_server_id=1
include_tables=tb1
```

5、启动maxwell，bin/maxwell --config config.properties
6、maxwell默认在源数据库生成库maxwell记录相关信息

7、测试Maxwell 是否正常解析MySQL的binlog日志

bin/maxwell --user='root' --password='root' --host='10.3.151.199' --producer='stdout'

## 7.Bireme 配置

1、下载地址：<https://github.com/HashDataInc/bireme/releases>

2、bireme官方文档：<https://github.com/HashDataInc/bireme/blob/master/README_zh-cn.md>
3、解压缩：tar xf bireme-1.0.0.tar.gz && cd bireme-1.0.0
4、修改配置文件，vim etc/config.properties

```
# target database where the data will sync into.
target.url = jdbc:postgresql://10.3.151.199:5432/syncdb
target.user = syncdb
target.passwd = syncdb

# data source name list, separated by comma.
data_source = maxwell1

# data source "mysql1" type
maxwell1.type = maxwell
# kafka server which maxwell write binlog into.
maxwell1.kafka.server = 127.0.0.1:9092
# kafka topic which maxwell write binlog into.
maxwell1.kafka.topic = mytopic
# kafka groupid used for consumer.
maxwell1.kafka.groupid = bireme

# set the IP address for bireme state server.
state.server.addr = 0.0.0.0
# set the port for bireme state server.
state.server.port = 8080
```

5、修改配置文件，vim etc/maxwell1.properties（**表映射配置**）
**note：maxwell1.properties的maxwell1一定要和bireme的data_source保持一致**

```
syncdb1.tb1 = public.tb1
syncdb2.tb1 = public.tb1
```

6、启动bireme，bin/bireme start
7、监控，http://10.3.151.174:8080/pretty （state.server.addr:state.server.port）

## 8.测试

1、mysql数据源

```
insert into tb1 select 1,'a';
insert into tb1 select 2,'b';
```

2、pgsql目标数据库

```
syncdb=# select * from tb1;
 a |     b      
---+------------
 1 | a         
 2 | b         
(2 rows)
```

## 9.总结

1、优势

- 可以实现多个库表的汇总功能，syncdb1.tb1/syncdb2.tb1 可以汇总到pgsql的一张表tb1中
- 中间使用kafka消息队列，对于大数据量性能方面提升较好

2、存在问题

- maxwell会在源数据库，生成maxwell库，用来存在相应binlog消费位点position信息，只能在数据源生成库

- 第一次启动maxwell时，只能以当前的binlog position位点开始解析，事先需要先同步数据源数据库到目标数据库的一份全量数据

  

#### 参考：

官方：https://hashdatainc.github.io/bireme/

Blog：http://blog.51cto.com/11257187/2153283