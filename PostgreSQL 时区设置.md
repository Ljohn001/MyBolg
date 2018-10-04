## PostgreSQL 时区设置

```

--有时候操作系统的时间与pg的时间不一致，这往往时两者时区不一致造成的  
  
--查看系统时间与时区  
[postgres@rudy_01 data]$ date  
Thu Nov 19 09:39:58 CST 2015  
[postgres@rudy_01 data]$ date -R  
Thu, 19 Nov 2015 09:40:33 +0800  
[postgres@rudy_01 data]$ cat /etc/sysconfig/clock  
ZONE="Asia/Shanghai"  
  
  
--查看pg的时区与时间  
postgres=# select now();  
              now                
-------------------------------  
 2015-11-18 17:42:28.755732-08  
(1 row)  
--查看时区  
postgres=# show time zone;  
  TimeZone    
------------  
 US/Pacific  
--以上可知，主机的时区和系统的时区不一致，造成两者相差16个小时   
   
   
 --修改时区，注意此默认为session级别  
 postgres=# set time zone 'PRC';  
SET  
postgres=# select now();  
              now                
-------------------------------  
 2015-11-19 09:44:50.178039+08  
(1 row)  
  
postgres=# show time zone;  
 TimeZone   
----------  
 PRC  
   
 --视图pg_timezone_names保存了所有可供选择的时区  
 select * from pg_timezone_names;  
   
--查看配置文件中时区设置，要想永久生效，此时需要修改配置文件
中国北京比GMT(格林威治标准时间)晚8个小时
设定:timezone='GMT-8:00'

[postgres@rudy_01 data]$ grep timezone postgresql.conf   
log_timezone = 'GMT-8:00'
timezone = 'GMT-8:00'  
  
  
--修改完配置时重新加载  
[postgres@rudy_01 ~]$ pg_ctl reload  
server signaled  
[postgres@rudy_01 ~]$ psql  
postgres=# show time zone;
 TimeZone 
----------
 GMT-8:00
(1 row)


```

