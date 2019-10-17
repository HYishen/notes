### 查看内核版本命令
uname -a

### 权限命令
chmod指令是更改文件读写执行权限的

chown指令是更改文件归属的,归属哪个用户,用户组是什么。对应将影响chmod里rwx效果。

### 关机命令
shutdown -h now

### 进程命令
ps

jobs //查看当前终端后台运行的任务
// Ctrl+Z用于将当前正在运行的前台进程暂停，变成后台进程。
// bg [%n]用于将后台暂停的进程继续运行。
// fg [%n]用于将后台执行的进程变成前台进程。
// kill %n用于杀掉指定的任务。

### HTTP请求命令
curl
wget

### 编译命令
make是用来编译的，它从Makefile中读取指令，然后编译。
make install是用来安装的，它也从Makefile中读取指令，安装到指定的位置。

### 查看文件内容命令
more
less
cat
head
tail

