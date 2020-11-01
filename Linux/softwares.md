### 网络配置工具
net-tools

iproute2

// 如今很多系统管理员依然通过组合使用诸如ifconfig、route、arp和netstat等命令行工具（统称为net-tools）来配置网络功能，解决网络故障。net-tools起源于BSD的TCP/IP工具箱，后来成为老版本Linux内核中配置网络功能的工具。但自2001年起，Linux社区已经对其停止维护。同时，一些Linux发行版比如Arch Linux和CentOS/RHEL 7则已经完全抛弃了net-tools，只支持iproute2。

### 查看网速
nethogs可以查看实时进程网络占用
安装： sudo apt install nethogs

查看网络状态： nethogs eth0

即 nethogs + 网卡名称

### xclip 粘贴板
xclip 命令可以从 stdin，或者文件读入数据到剪贴板，或者将剪贴板内容粘贴到目的应用中。xclip 命令建立了终端和剪切板之间通道，可以用命令的方式将终端输出或文件的内容保存到剪切板中，也可以将剪切板的内容输出到终端或文件

```
// 安装
sudo apt-get xclip
```

```
https://en.wikipedia.org/wiki/X_Window_selection

在 X 系统里面，从一个窗口复制一段文字到另一个窗口，有两套机制，分别是 Selections 和 cut buffers。

常用的 copy & paste 是利用的 cut buffers 机制;另外用鼠标选中一段文字，然后在另一个窗口按鼠标中键实现复制，利用的是 selections 机制。selection 又可以分为 master 和 slave selection。

当用鼠标选中一段文件，这段文字就自动被复制到 master selection。然后在另一个地方按鼠标中键，就自动把 master selection 的内容粘贴出来。
```

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
