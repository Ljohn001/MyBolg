### 客户端报错：

```
在进行导数据时，发现GP异常，登录Master 发现
DETAIL:  FATAL:  Out of memory
DETAIL:  VM protect failed to allocate 16872 bytes from system, VM Protect 8187 MB available
```

```
DETAIL:  could not connect to server: Connection refused
        Is the server running on host "10.64.104.138" and accepting
        TCP/IP connections on port 40001?
 (seg1 10.64.104.138:40001)
```

登录GP，发现一个节点segment挂了

```
[gpadmin@gpmaster137 ~]$ gpstate  -m 
20180905:14:58:07:619685 gpstate:gpmaster137:gpadmin-[INFO]:-Starting gpstate with args: -m
20180905:14:58:07:619685 gpstate:gpmaster137:gpadmin-[INFO]:-local Greenplum Version: 'postgres (Greenplum Database) 5.10.1 build commit:b3c02f3acd880e2d676dacea36be015e4a3826d4'
20180905:14:58:07:619685 gpstate:gpmaster137:gpadmin-[INFO]:-master Greenplum Version: 'PostgreSQL 8.3.23 (Greenplum Database 5.10.1 build commit:b3c02f3acd880e2d676dacea36be015e4a3826d4) on x86_64-pc-linux-gnu, compiled by GCC gcc (GCC) 6.2.0, 64-bit compiled on Aug  2 2018 22:37:4
5'
20180905:14:58:07:619685 gpstate:gpmaster137:gpadmin-[INFO]:-Obtaining Segment details from master...
20180905:14:58:07:619685 gpstate:gpmaster137:gpadmin-[WARNING]:--------------------------------------------------------------
20180905:14:58:07:619685 gpstate:gpmaster137:gpadmin-[WARNING]:-physical mirroring not used
20180905:14:58:07:619685 gpstate:gpmaster137:gpadmin-[WARNING]:--------------------------------------------------------------
[gpadmin@gpmaster137 ~]$ gpstate  -s 
20180905:14:58:10:619719 gpstate:gpmaster137:gpadmin-[INFO]:-Starting gpstate with args: -s
20180905:14:58:10:619719 gpstate:gpmaster137:gpadmin-[INFO]:-local Greenplum Version: 'postgres (Greenplum Database) 5.10.1 build commit:b3c02f3acd880e2d676dacea36be015e4a3826d4'
20180905:14:58:10:619719 gpstate:gpmaster137:gpadmin-[INFO]:-master Greenplum Version: 'PostgreSQL 8.3.23 (Greenplum Database 5.10.1 build commit:b3c02f3acd880e2d676dacea36be015e4a3826d4) on x86_64-pc-linux-gnu, compiled by GCC gcc (GCC) 6.2.0, 64-bit compiled on Aug  2 2018 22:37:4
5'
20180905:14:58:10:619719 gpstate:gpmaster137:gpadmin-[INFO]:-Obtaining Segment details from master...
20180905:14:58:10:619719 gpstate:gpmaster137:gpadmin-[INFO]:-Gathering data from segments...
. 
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-----------------------------------------------------
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:--Master Configuration & Status
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-----------------------------------------------------
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[WARNING]:-*****************************************************
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[WARNING]:-DATABASE IS PROBABLY UNAVAILABLE
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[WARNING]:-Review Instance Status in log file or screen output for more information
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[WARNING]:-*****************************************************
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Master host                    = gpmaster137
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Master postgres process ID     = 619332
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Master data directory          = /data/gpmaster/gpseg-1
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Master port                    = 5432
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Master current role            = dispatch
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Greenplum initsystem version   = 5.10.1 build commit:b3c02f3acd880e2d676dacea36be015e4a3826d4
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Greenplum current version      = PostgreSQL 8.3.23 (Greenplum Database 5.10.1 build commit:b3c02f3acd880e2d676dacea36be015e4a3826d4) on x86_64-pc-linux-gnu, compiled by GCC gcc (GCC) 6.2.0, 64-bit compiled on Aug  2 201
8 22:37:45
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Postgres version               = 8.3.23
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Master standby                 = gpsegment138
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Standby master state           = Standby host passive
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-----------------------------------------------------
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-Segment Instance Status Report
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-----------------------------------------------------
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Segment Info
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-      Hostname                          = gpsegment138
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-      Address                           = gpsegment138
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-      Datadir                           = /data/primary1/gpseg0
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-      Port                              = 40000
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-   Status
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[WARNING]:-   PID                               = Not found                                       <<<<<<<<
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-      Configuration reports status as   = Up
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[WARNING]:-   Database status                   = Process error -- database process may be down   <<<<<<<<
20180905:14:58:11:619719 gpstate:gpmaster137:gpadmin-[INFO]:-----------------------------------------------------
```



解决办法：

**如上报错，原因是内存限制参数没有配置导致的。**

greenplum可用通过配置gp_vmem_protect_limit来限制单个segment节点的内存使用。

通过配置statement_mem和max_statement_mem来限制单条SQL的内存使用。

```
8GB 交换空间 SWAP
128GB 内存 RAM
vm.overcommit_ratio = 50 === 0.5 
8个段数据库
(8 + (128*0.5) )*0.9/8 = 8GB
则设置 gp_vmem_protect_limit = 8GB, 需要转换为 8192MB
```

```
//在Master配置
gpconfig -c gp_vmem_protect_limit -v 8192	//根据如上计算方式换算值
```



### 参考

https://yq.aliyun.com/articles/225816