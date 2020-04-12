### 网络配置工具
net-tools

iproute2

// 如今很多系统管理员依然通过组合使用诸如ifconfig、route、arp和netstat等命令行工具（统称为net-tools）来配置网络功能，解决网络故障。net-tools起源于BSD的TCP/IP工具箱，后来成为老版本Linux内核中配置网络功能的工具。但自2001年起，Linux社区已经对其停止维护。同时，一些Linux发行版比如Arch Linux和CentOS/RHEL 7则已经完全抛弃了net-tools，只支持iproute2。

### 查看网速
nethogs可以查看实时进程网络占用
安装： sudo apt install nethogs

查看网络状态： nethogs eth0

即 nethogs + 网卡名称

### 远程桌面连接
remmina

Vinagre

### 文本形式浏览网站
w3m

### 查看CPU温度
sudo apt-get install lm-sensors

使用命令：

sensors

### 截图软件
sudo apt install flameshot

flameshot
