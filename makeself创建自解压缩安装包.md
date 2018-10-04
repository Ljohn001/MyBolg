## Linux 下Makeself创建自解压缩安装包

1、介绍：Makeself.sh是一个小的Shell脚本。用于从一个文件夹中生成自解压的tar.gz压缩包。结果文件以一个shell脚本显示（大多数以.bin作为后缀名）。能够自己主动执行。该文档会解压自己到一个暂时文件夹，而且执行一个可选的随意命令（比如：一个安装脚本）。它很类似于Windows中的Winzip自解压生成的文件。Makeself文档也包含校验和用于集成子验证（CRC或MD5校验和）。

2、下载地址：https://github.com/megastep/makeself/releases/download/release-2.4.0/makeself-2.4.0.run

3、官方：http://makeself.io/

4、安装：

```
# wget https://github.com/megastep/makeself/releases/download/release-2.4.0/makeself-2.4.0.run
# chmod 755 makeself-2.2.0.run
#./makeself-2.4.0.run
# cd makeself-2.4.0
# cp *.sh /usr/local/bin/
```

5、用法

```
makeself.sh [args] archive_dir file_name label startup_script [script_args]
# 注释
archive_dir：包括归档文件的文件夹名称
file_name：创建归档文件的名称
label：描写叙述软件包的随意文本字符串，当解压文件时显示
startup_script：在提取文件文件夹中的命令，因此假设你希望运行一个
```

args* are optional options for Makeself. The available ones are :

- **--version** : Prints the version number on stdout, then exits immediately
- **--gzip** : Use gzip for compression (the default on platforms on which gzip is commonly available, like Linux)
- **--bzip2** : Use bzip2 instead of gzip for better compression. The bzip2 command must be available in the command path. It is recommended that the archive prefix be set to something like ‘.bz2.run’, so that potential users know that they’ll need bzip2 to extract it.
- **--pbzip2** : Use pbzip2 instead of gzip for better and faster compression on machines having multiple CPUs. The pbzip2 command must be available in the command path. It is recommended that the archive prefix be set to something like ‘.bz2.run’, so that potential users know that they’ll need bzip2 to extract it.
- **--xz** : Use xz instead of gzip for better compression. The xz command must be available in the command path. It is recommended that the archive prefix be set to something like ‘.xz.run’ for the archive, so that potential users know that they’ll need xz to extract it.
- **--lzo** : Use lzop instead of gzip for better compression. The lzop command must be available in the command path. It is recommended that the archive prefix be set to something like `.lzo.run` for the archive, so that potential users know that they’ll need lzop to extract it.

....

> 更多参数参考官方文档

6、创建自解压文件（实例）

```
makeself.sh  ./ptuan ptuan.bin ptuan /usr/local/bin/install.sh
# 注释：
./ptuan 表示当前目录下文件ptuan文件夹
ptuan.bin 表示生成的文件
ptuan 表示标签
/usr/local/bin/install.sh 解压后执行的脚本
```

install.sh 文件

```
#!/bin/bash
#
echo "Upload start ..."

CURRENT_DIR=`pwd`
echo "1. Current directory$CURRENT_DIR"
INSTALL_DIR=/opt/backup/www/wwwroot/vphotos.cn

echo "2. Backup directory$INSTALL_DIR"
/bin/bash /usr/local/bin/backup.sh &> /dev/null

echo "3. Check the directory:$INSTALL_DIR"
[ ! -d $INSTALL_DIR ] && mkdir -p $INSTALL_DIR

echo "4. Move file to$INSTALL_DIR"
rsync -avr --delete --exclude='database.php' --exclude='config.php' --exclude='upload' --exclude='log' * $INSTALL_DIR &>/dev/null
chown -R www.www $INSTALL_DIR
chmod -R  777 $INSTALL_DIR

echo "5. Delete the directory:$CURRENT_DIR"
cd $INSTALL_DIR
rm -rf $CURRENT_DIR

echo "Update done ..."

```

7、安装自解压缩包

```
# chmod +x ptuan-master-1809130855.bin
# ./ptuan-master-1809130855.bin 
Verifying archive integrity...  100%   All good.
Uncompressing ptuan  100%  
Upload start ...
1. Current directory/tmp/selfgz1891422779
2. Backup directory/opt/backup/www/wwwroot/vphotos.cn
3. Check the directory:/opt/backup/www/wwwroot/vphotos.cn
4. Move file to/opt/backup/www/wwwroot/vphotos.cn
5. Delete the directory:/tmp/selfgz1891422779
Update done ...

```

