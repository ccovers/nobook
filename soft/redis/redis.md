# redis

## redis 介绍
Redis 是完全开源免费的，遵守BSD协议，是一个高性能的key-value数据库
Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用
Redis不仅仅支持简单的key-value类型的数据，同时还提供list，set，zset，hash等数据结构的存储
Redis支持数据的备份，即master-slave模式的数据备份

## 命令
```
Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)
```

### string
string是redis最基本的类型，你可以理解成与Memcached一模一样的类型，一个key对应一个value
string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象
string类型是Redis最基本的数据类型，一个键最大能存储512MB
```
SET key value
GET key
```

### hash
Redis hash 是一个键值对集合。
Redis hash是一个string类型的field和value的映射表，hash特别适合用于存储对象
```
HMSET key field value [field value ...]
HGET key field
HGETALL key
```

### list
Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边）
```
LPUSH key value [value ...]
LRANGE key start stop
```

### set
Redis的Set是string类型的无序集合
集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是O(1)
```
SADD key member [member ...]
SMEMBERS key
```

### zset (sorted set: 有序集合)
Redis zset 和 set 一样也是string类型元素的集合，且不允许重复的成员
不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序
zset的成员是唯一的,但分数(score)却可以重复
```
ZADD key score member [score member ...]
ZRANGE key start stop
```




