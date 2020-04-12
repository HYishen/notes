### 服务端
```
启动zookeeper：bin/zkServer.sh start

查看状态：bin/zkServer.sh status

停止zookeeper：bin/zhServer.sh stop
```

### 客户端
```
启动客户端：bin/zkCli.sh
```

客户端命令行操作

| 命令基本语法     | 功能描述                                         |
| ---------------- | ------------------------------------------------ |
| help             | 显示所有操作命令                                 |
| ls path [watch]  | 使用ls命令来查看当前ZNode中所包含的内容          |
| ls2 path [watch] | 查看当前节点详细数据并能看到更新次数等数据           |
| create           | 普通创建  -s 含有序列 -e临时（重启或者超时消失） |
| get path [watch] | 获得节点的值                                     |
| set              | 设置节点的具体值                                 |
| stat             | 查看节点状态                                     |
| delete           | 删除节点                                         |
| rmr              | 递归删除节点                                     |

加[watch]表示监听某个节点的值，注册的监听只监听一次的变化，之后的变化不会有反应
