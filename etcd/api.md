# api

## etcd客户端api
git clone https://github.com/etcd-io/etcd.git

## 创建键值
curl http://127.0.0.1:2379/v2/keys/cqh -XPUT -d value="小小"

## 创建目录
curl http://127.0.0.1:2379/v2/keys/gym -XPUT -d dir=true

##获取键值
curl http://127.0.0.1:2379/v2/keys/cqh

## 创建键值带ttl
curl http://127.0.0.1:2379/v2/keys/hero -XPUT -d value="超人" -d ttl=5

## 创建有序键值
curl http://127.0.0.1:2379/v2/keys/fitness -XPOST -d value="bench_press"
curl http://127.0.0.1:2379/v2/keys/fitness -XPOST -d value="dead_lift"
curl http://127.0.0.1:2379/v2/keys/fitness -XPOST -d value="deep_squat"

## 获取刚创建的fitness
curl http://127.0.0.1:2379/v2/keys/fitness

## 删除键
curl http://127.0.0.1:2379/v2/keys/cqh -XDELETE

## 列出所有集群成员
curl http://127.0.0.1:2379/v2/members

## 统计信息-查看leader
curl http://127.0.0.1:2379/v2/stats/leader

## 节点自身信息
curl http://127.0.0.1:2379/v2/stats/self

## 查看集群运行状态
curl http://127.0.0.1:2379/v2/stats/store






