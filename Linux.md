- `vmstat` 和 `iostat` 命令用来观察服务器详细状态, 可使用`dstat`命令代替`vmstat`  能输出比 `vmstat` 更加：美观，整洁，强大的内容 

-  `uptime`  查看系统瓶颈负载`load average`就表示系统最近 `1 分钟、5 分钟、15 分钟`的系统瓶颈负载。 

-  `lscpu` 查看 CPU 信息 

-  `cat /proc/cpuinfo`查看每个 `CPU` 核的信息 

-  `stress`是一个施加系统压力和压力测试系统的工具，我们可以使用`stress`工具压测试 `CPU`，以便方便我们定位和排查 `CPU` 问题 

-  `mpstat`使用`mpstat -P ALL 1`则可以查看每一秒的 CPU 每一核变化信息，整体和`top`类似，好处是可以把每一秒（自定义）的数据输出方便观察数据的变化，最终输出平均数据 

- `pidstat` 用于查看全部或指定进程的`cpu`、内存、线程、设备`IO`等系统资源的占用情况 

- ```
  pidstat -urd -h -p 18225 5 (-urd)查看cpu,内存,io三个指标, (-h)内存，cpu 、io的信息合并在一栏
  ```

- `pidof` 查找指定名称的进程的进程号

- `cat /etc/passwd` 查看系统所有用户  

- `cat /etc/group` 查看系统所有组 

- `lsof` 查看进程打开的文件,  参数 `-p + 进程号`(查看某个进程打开的文件) `lsof | grep mysql` 查看`mysql`打开的文件  

-  `free`  查看系统内存使用情况 , `free 参数 -b(以Bety为单位) -k(以kb为单位) -m(MB为单位) -g(以GB为单位) 展示`  -s<间隔秒>持续观察内存使用情况  -h ：以人们较易阅读的 `GBytes, MBytes, KBytes` 等格式自行显示

- `df`  查看磁盘使用情况 

- 文件压缩与解压缩命令

  ```
  tar -zcf test.tar.gz test.txt 将文件压缩
  tar -zxf test.tar.gz 将文件杰压缩
  tar -cf all.tar *.jpg 
  这条命令是将所有 .jpg 的文件打成一个名为 all.tar 的包。-c 是表示产生新的包，-f 指定包的文件名。
  tar -rf all.tar *.gif
  这条命令是将所有 .gif 的文件增加到 all.tar 的包里面去，-r 是表示增加文件的意思。 
  ```

-  如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 `/dev/null`： 

  ```
  $ command > /dev/null
  ```

-  `/dev/null` 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 `/dev/null` 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。 如果希望屏蔽 `stdout` 和 `stderr`，可以这样写 

  ```
  $ command > /dev/null 2>&1
  ```

-  `netstat -pnltu` 查看服务及它们所监听的端口

-  下例命令解释: 查找/var/log目录中更改时间在7日以前的普通文件，并删除它们 

  ```
  find /var/log -type f -mtime +7 -exec rm {} \;
  ```

- `ls -lht`   查看当前文件夹下各文件夹大小 

-  查看当前文件夹下各文件夹大小  

  ```
  du -h --max-depth=1
  du -sh `ls`
  du -sh /* 递归查看
  ```

-  环境变量设置 

  ```
  echo $PATH  查看环境变量
  env  查看环境变量
  vim /etc/profile  编辑环境变量
  vim .bash_profile 编辑环境变量
  ```

-  磁盘管理 

  ```
  1: fdisk 磁盘管理命令通过这个命令来添加分区和分配分区的大小
  2: mkfs  通过这个命令来格式化分区后分区才能使用
  3: mount 使用这个命令将分区挂载到指定的文件夹上
  4: vim /etc/fstab 将挂载信息永久记录,使用mount命令挂载的分区服务器重启后会失效,所以需要在
  /etc/fstab配置文件中记录挂载信息使其永久有效
  
  ```

- `lsblk -f` 查看系统分区情况 

- 通过`cat`命令快捷创建文件并输入内容 

  ```
  1. cat > test.php
  2. 输入内容
  3. Ctrl + d 保存
  ```

  