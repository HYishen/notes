### 权限文件
#### 系统用户信息
/etc/passwd

文件格式：
```
Name:password:ID:group ID:comment:home directory:login shell
用户名:密码:ID:用户组ID:注释:家目录:登录使用的shell
```

#### 系统用户组信息
/etc/group

文件格式：
```
Group name:password:group ID:user list
用户组名:密码:用户组ID:用户列表
```

#### 用户密码加密后的文件
/etc/shadow

安全性要求较高的Linux系统都把加密后的口令字分离出来，单独存放在一个文件中，这个文件是/etc/shadow文件。

/etc/shadow中的记录行与/etc/passwd中的一一对应，它由pwconv命令根据/etc/passwd中的数据自动产生

内容格式：
```
登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志
```

### 输入输出类文件
#### /dev/null 文件
/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。

如果希望屏蔽 stdout 和 stderr，可以这样写：
```
command > /dev/null 2>&1
```

### 配置类文件
#### /etc/hosts 文件
hosts文件是linux系统中负责ip地址与域名快速解析的文件，以ASCII格式保存在/etc目录下，文件名为hosts，不同的linux版本，文件也可能不同，比如Debian的对应文件是/etc/hostname。hosts文件包含了ip地址和主机名之间的映射，包括主机名的别名，在没有域名服务器的情况下，系统上的所有网络程序都通过查询该文件来解析对应于某个主机名的ip地址，否则就需要使用DNS服务程序来解决。通常可以将常用的域名和ip地址映射加入到hosts文件中，实现快速方便的访问

优先级：dns缓存 \> hosts \> dns服务

hosts：the static table lookup for host name（主机名查询静态表）

### debian或ubuntu的应用程序配置目录
/usr/share/applications

配置例子：
```
// firefox.desktop
[Desktop Entry]
Name=firefox
Name[zh_CN]=火狐浏览器
Comment=火狐浏览器
Exec=/opt/firefox/firefox
Icon=/opt/firefox/browser/chrome/icons/default/default128.png
Terminal=false
Type=Application
Categories=Appliction;
Encoding=UTF-8
StartupNotify=true
```
