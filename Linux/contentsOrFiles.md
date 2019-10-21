### 系统用户信息
/etc/passwd

文件格式：
```
Name:password:ID:group ID:comment:home directory:login shell
用户名:密码:ID:用户组ID:注释:家目录:登录使用的shell
```

### 系统用户组信息
/etc/group

文件格式：
```
Group name:password:group ID:user list
用户组名:密码:用户组ID:用户列表
```

### 用户密码加密后的文件
/etc/shadow

安全性要求较高的Linux系统都把加密后的口令字分离出来，单独存放在一个文件中，这个文件是/etc/shadow文件。

/etc/shadow中的记录行与/etc/passwd中的一一对应，它由pwconv命令根据/etc/passwd中的数据自动产生

内容格式：
```
登录名:加密口令:最后一次修改时间:最小时间间隔:最大时间间隔:警告时间:不活动时间:失效时间:标志
```