## Greenplum备份与恢复

目前GP支持的备份方式有：

1）pg_dump非并行备份，备份文件存放在master上，适合数据量小的备份场景，简单易用；



2）gpcrondump并行备份，master备份元数据，计算实例并行备份各自的数据；

3）冷备，停库后，将master和instance的数据文件夹统一备份存放。











### FAT

Q: pg_dump备份报错

```
 ~]$ pg_dump vphotos  -Ugpadmin -W -f vphotos.dump                                                                                                                                   
Password: 
pg_dump: SQL command failed
pg_dump: Error message from server: ERROR:  bogus varattno for OUTER var: 8 (ruleutils.c:3237)
pg_dump: The command was: SELECT c.relname, pr.parname,CASE WHEN p.parkind <> 'r'::char OR pr.parisdefault THEN NULL::bigint ELSE pg_catalog.rank() OVER ( PARTITION BY p.oid, cc.relname, p.parlevel, cl3.relname ORDER BY pr.parisdefault, pr.parruleord) END AS partitionrank FROM pg_partition p JOIN pg_class cc ON (p.parrelid = cc.oid), pg_class c JOIN pg_partition_rule pr ON (c.oid = pr.parchildrelid) LEFT JOIN pg_partition_rule pr2 ON pr.parparentrule = pr2.oid LEFT JOIN pg_class cl3 ON pr2.parchildrelid = cl3.oid WHERE pr.paroid = p.oid AND p.parrelid = 1319995 AND c.relstorage = 'x';
```

A: 

