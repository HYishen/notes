### 创建并运行一个名为 myredis 的容器
docker run \
-p 6379:6379 \
-v $PWD/data:/data \
-v $PWD/redis.conf:/etc/redis/redis.conf \
--privileged=true \
--name myredis \
-d redis:6.0 redis-server /etc/redis/redis.conf

### 命令分解
docker run \
-p 6379:6379 \ # 端口映射 宿主机:容器
-v $PWD/data:/data:rw \ # 映射数据目录 rw 为读写
-v $PWD/redis.conf:/etc/redis/redis.conf:ro \ # 挂载配置文件 ro 为readonly
--privileged=true \ # 给与一些权限
--name myredis \ # 给容器起个名字
-d redis redis-server /etc/redis/redis.conf # deamon 运行容器 并使用配置文件启动容器内的 redis-server

### 外部访问 redis 容器服务
```
// 以bash的方式进入到redis容器内部
docker exec -it myredis bash
// 启动redis客户端
redis-cli
// 在redis客户端中输入密码来验证
auth {password}
```
