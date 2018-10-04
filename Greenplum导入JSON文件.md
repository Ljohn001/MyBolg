## Greenplum导入JSON文件

### JSON与表

JSON作为一个半结构化的数据格式，很容易实现与结构化的数据库表结构之间的相互转换；我们可以参考下面一个简单的例子。

```
{ "name":"John", "age":30, "car":null }
```

显而易见它可以跟如下的表建立一一对应的映射关系：

```
create table person (name text, age int, car text);
```

在当用户需要把JSON格式的数据转换为数据库的记录时，每一个新的JSON对象可以理解为向表中插入新的一行数据：

```
insert into person values ("John", 30, NULL);
```

当用户有大量的JSON数据需要导入时，一个个的insert操作性能会很差。由于Greenplum的批量导入工具只支持text和csv两种文本格式（即使算上GPHDFS，也只有AVRO和parquet两种），直接导入JSON似乎并不是那么容易。 但是，只要需要导入的JSON数据格式满足特定的条件，利用Greenplum已有的工具和特性也是可以方便的通过外部表的方式批量导入JSON数据的。

### JSON的文件格式

JSON标准本身只是定义了对象的表示方式，因此JSON对象的保存和传输时需要一些额外的处理。如果把整个文件当成一个大的JSON对象，在把整个文件完全读完之前是无法对其进行处理的，这样既消耗系统内存也不便于流式处理。现在最常见的办法是以行为单位保存保存每个JSON对象，整个文件就是以换行符分割的JSON对象列表。接下来就以这种格式的JSON文件为例，演示如何将其导入Greenplum中。使用的JSON文件内容如下。

```
{ "id": 1, "type": "home", "address": { "city": "Boise", "state": "Idaho" }}
{ "id": 2, "type": "fax", "address": { "city": "San Francisco", "state": "California" }}
{ "id": 3, "type": "cell", "address": { "city": "Chicago", "state": "Illinois" }
```



### Greenplum 5的导入方法

首先，由于Greenplum5原生的支持了JSON类型，因此有了更便捷的方式导入JSON文件，例子如下：

```
create external table ext_json (data json) location ('gpfdist://mdw:8080/sample.json') format 'text';
create table json_data ( id int, type text, city text) ;
```

利用内置的JSON操作符，通过如下命令即可完成JSON的导入

```
insert into json_data select data->'id', data->'type', data->'address'->'city' as city from ext_json;
```

由于是原生支持了JSON格式，这种方法适用于所有的Greenplum外表。需要注意的是外部表在进行行和列的切割是，会检查指定的换行和转义符号，因此尽量选择一个不会出现的符号当作转义符和列分隔符，可以参考下面的查询，指定ASCII的0x1E和0x1F来作为转义和分隔符。

```
create external table ext_json (data json) location ('gpfdist://mdw:8080/sample.json') format 'text' (ESCAPE E'\x1E' DELIMITER E'\x1F');
```

### 支持RFC7464

JSON的文件格式，除了这里使用的行分割的文件外，还有一种“JSON文本序列”的格式，它也是RFC7464定义的一种对象序列化格式。对每个JSON对象它定义了开始和结束两个标志，结束标志也使用的换行符，开始标志使用了0x1E(“Record Separator”)字符。其格式为RS{JSON_OBJECT}LF。 对这种格式JSON文件，可以通过指定转义字符的方式，忽略其开始标志。

```
create external table ext_json (data json) location ('gpfdist://mdw:8080/example.log') format 'text' (ESCAPE E'\x1E');
```

这是因为任何JSON对象都是以{符号开始的，而转义后的{仍然是它自己。

### 小节

这里介绍了几种常用的向Greenplum中导入JSON数据的方式，由于Greenplum 5增加了原生的JSON格式支持，因此可以直接对外部的JSON文件进行复杂的解析操作，一步到位的完成数据的转换和加载。如果这些JSON文件保存在HDFS上，Greenplum5还可以通过PXF来导入，关于PXF以后会有专门的介绍。

  





参考文档

https://digitx.cn/2018/02/02/greenplum_json/

http://dewoods.com/blog/greenplum-json-guide