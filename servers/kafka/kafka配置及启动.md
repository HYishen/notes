#### 启动kafka
```
bin/kafka-server-start.sh [-daemon] config/server.properties
```

#### 关闭kafka
```
bin/kafka-server-stop.sh
```

#### 使用docker-compose来启动
编写docker-compose.yml文件
```
version: '2'
services:
  zookeeper:
    image: wurstmeister/zookeeper
    ports:
      - "2181:2181"
  kafka:
    image: wurstmeister/kafka                       ## 镜像
    ports:
      - "9092"
    environment:
      KAFKA_ADVERTISED_HOST_NAME: 10.168.1.188      ## 修改:宿主机IP
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
  kafka-manager:
    image: sheepkiller/kafka-manager                ## 镜像：开源的web管理kafka集群的界面
    environment:
        ZK_HOSTS: 10.168.1.188                      ## 修改:宿主机IP
    ports:
      - "9000:9000"                                 ## 暴露端口
```
后台启动
```
docker-compose up -d
```
设置kafka节点可以使用
```
docker-compose scale kafka=[number]
```