# etcd

## 介绍
分布式系统中，服务的配置信息的管理分享和服务的发现
重启：systemctl restart etcd.service

## 命令
### 设置键值
etcdctl set key "hello world"
```
--sort    对结果进行排序
--consistent 将请求发给主节点，保证获取内容的一致性
```

### 修改键值
etcdctl update key "hahaha"
```
--ttl '0'    超时时间（单位为秒），不配置（默认为 0）则永不超时
```

### 获取键值
etcdctl get key
```
--sort    对结果进行排序
--consistent 将请求发给主节点，保证获取内容的一致性
```
通过http访问: curl -L http://localhost:2379/v2/keys/key

### 删除键值
 etcdctl rm key
```
--dir        如果键是个空目录或者键值对则删除
--recursive        删除目录和所有子键
--with-value     检查现有的值是否匹配
--with-index '0'    检查现有的 index 是否匹配
```

### 键不存在则创建新的键
etcdctl mk key "Hello world"
```
--ttl '0'    超时时间（单位为秒），不配置（默认为 0）则永不超时
```

### 键目录不存在则创建一个新的键目录
etcdctl mkdir /testdir
```
--ttl '0'    超时时间（单位为秒），不配置（默认为 0）则永不超时
```

### 创建一个键目录，无论存在与否
etcdctl setdir /testdir
```
--ttl '0'    超时时间（单位为秒），不配置（默认为 0）则永不超时
```

### 更新一个已经存在的目录
etcdctl updatedir /testdir
```
--ttl '0'    超时时间（单位为秒），不配置（默认为 0）则永不超时
```

### 删除一个空目录，或者键值对（若目录不空，会报错）
etcdctl rmdir /testdir

### 列出目录（默认为根目录）下的键或者子目录，默认不显示子目录中内容
etcdctl ls /testdir
```
--sort    将输出结果排序
--recursive    如果目录下有子目录，则递归输出其中的内容
-p        对于输出为目录，在最后添加 `/` 进行区分
```


## 非数据库操作
### 备份 etcd 的数据
backup
```
--data-dir         etcd 的数据目录
--backup-dir     备份到指定路径
```

### 监测一个键值的变化，一旦键值发生更新，就会输出最新的值并退出
etcdctl watch key -f
```
--forever/-f        一直监测，直到用户按 `CTRL+C` 退出
--after-index '0'    在指定 index 之前一直监测
--recursive        返回所有的键值和子键值
```

### 监测一个键值的变化，一旦键值发生更新，就执行给定命令
etcdctl exec-watch key -- sh -c 'pwd'
```
--after-index '0'    在指定 index 之前一直监测
--recursive        返回所有的键值和子键值
```

### 通过 list、add、remove 命令列出、添加、删除 etcd 实例到 etcd 集群中
member
例如本地启动一个 etcd 服务实例后，可以用如下命令进行查看。
```
$ etcdctl member list
ce2a822cea30bfca: name=default peerURLs=http://localhost:2380,http://localhost:7001 clientURLs=http://localhost:2379,http://localhost:4001
```

## 命令选项
--debug 输出 cURL 命令，显示执行命令的时候发起的请求
--no-sync 发出请求之前不同步集群信息
--output, -o 'simple' 输出内容的格式 (simple 为原始信息，json 为进行json格式解码，易读性好一些)
--peers, -C 指定集群中的同伴信息，用逗号隔开 (默认为: “127.0.0.1:4001”)
--cert-file HTTPS 下客户端使用的 SSL 证书文件
--key-file HTTPS 下客户端使用的 SSL 密钥文件
--ca-file 服务端使用 HTTPS 时，使用 CA 文件进行验证
--help, -h 显示帮助命令信息
--version, -v 打印版本信息


