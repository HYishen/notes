### 查看目录下的文件
ls

使用正则表达式查找某个目录下的文件
```
// 查找ls /usr/bin/目录下以.sh结尾的文件
ls /usr/bin/*.sh
```

### 作为参数传给下一个命令
xargs
```
xargs 是一个强有力的命令，它能够捕获一个命令的输出，然后传递给另外一个命令。

命令格式：

somecommand |xargs [-item]  command
```

### 查看内核版本命令
uname -a

### 关机命令
shutdown -h now

### 进程命令
ps

jobs //查看当前终端后台运行的任务<br/>
// Ctrl+Z用于将当前正在运行的前台进程暂停，变成后台进程。<br/> 
// bg [%n]用于将后台暂停的进程继续运行。<br/> 
// fg [%n]用于将后台执行的进程变成前台进程。<br/> 
// kill %n用于杀掉指定的任务。

### top命令
top可以查看进程和线程在运行过程中占用的系统资源

top命令可以实时显示各个线程情况。要在top输出中开启线程查看，请调用top命令的“-H”选项，该选项会列出所有Linux线程。在top运行时，你也可以通过按“H”键将线程查看模式切换为开或关。
```
top -H
```

要让top输出某个特定进程<pid>并检查该进程内运行的线程状况：
```
top -H -p <pid>
```

### HTTP请求命令
curl
wget

### 编译命令
make是用来编译的，它从Makefile中读取指令，然后编译。<br/> 
make install是用来安装的，它也从Makefile中读取指令，安装到指定的位置。

### 查找文件命令
find

常用参数：

-name

如：
```
find /etc -name .bashrc
```

### 查看文件内容命令
more

less

cat

head

tail

tac  从最后一行开始显示，可以看出 tac 是 cat 的倒著写！

nl   显示的时候，顺道输出行号！

### 重定向类
#### tee
1.读取标准输入的数据，并将其内容输出成文件
2.主要用于重定向到文件

常用参数
```
-a，将读取的内容追加到文件的后面，而不是覆盖（在默认的情况下是覆盖）
```

命令tee与重定向的区别
```
重定向，是将读取的内容输出到指定文件中，在屏幕上并不显示
命令tee，在屏幕上显示的同时，将读取的内容也重定向到指定文件中
```

例子：
```
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://xxxxxxxx.mirror.aliyuncs.com"]
}
EOF
```

### 查看网络连接、端口占用等信息
netstat
(常用选项：-anp)

### 磁盘管理
df 检查文件系统的磁盘空间占用情况

fdisk 磁盘分区表操作工具

mkfs 磁盘格式化
```
mkfs [-t 文件系统格式] 装置文件名
```

fsck 磁盘检验 // 用来检查和维护不一致的文件系统。若系统掉电或磁盘发生问题，可利用fsck命令对文件系统进行检查。

mount 磁盘挂载

umount 磁盘卸除

#### du 估算文件占用空间 
du 对文件和目录磁盘使用的空间的查看

```
// 查看当天目录第1层文件所占大小
// -d 文件层级深度 详情看 man du
du -hd 1
```

#### tree 以树形结构显示文件目录结构
```
tree 以树状图显示所有文件

tree -L N 以树状图显示所有文件，子文件夹显示到第 N 层
```

```
// 文件下载命令
sudo apt-get install tree
```

### 检查那些进程使用了某个文件
fuser命令
fuser能查出谁在使用这个资源

```
// 该命令能查出有那些进程在使用 /var/cache/debconf/config.dat这个文件
sudo fuser -v /var/cache/debconf/config.dat

在查出有那些进程使用这个文件时，有必要就使用
sudo kill PID
sudo kill -9 PID  # if the first doesn't work
来杀掉这个进程
```
