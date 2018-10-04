修改GP参数启动不了

```
gpconfig -c gp_vmem_protect_limit -v 8192MB　//错误写法
```

```
gpconfig -c gp_vmem_protect_limit -v 8192	//正确写法
```

```
gpconfig -c max_statement_mem -v 16384MB
gpconfig -c statement_mem -v 2400MB
```

