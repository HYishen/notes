查看一个数据库中的keys
```
keys *
```

选择数据库
```
select [number]

// 选择相应的数据库之后，命令行会自动显示当前所在数据库编号
// 如 select 1之后，命令行变为：127.0.0.1:6379[1]>
```

为给定 key 设置过期时间，以秒计
```
EXPIRE key seconds
```

设置 key 的过期时间以毫秒计。
```
PEXPIRE key milliseconds
```

以秒为单位，返回给定 key 的剩余生存时间(TTL, time to live)。
```
// 当 key 不存在时，返回 -2 。 当 key 存在但没有设置剩余生存时间时，返回 -1 。 否则，以秒为单位，返回 key 的剩余生存时间。
// 注意：在 Redis 2.8 以前，当 key 不存在，或者 key 没有设置剩余生存时间时，命令都返回 -1 。

TTL KEY_NAME
```

以毫秒为单位返回 key 的剩余的过期时间。
```
PTTL key
```

仅当 newkey 不存在时，将 key 改名为 newkey 。
```
RENAMENX key newkey
```

修改 key 的名称
```
RENAME key newkey
```
