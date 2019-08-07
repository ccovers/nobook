# kafaka

## `kafka`成员
1. Producer（生产者）

2. Broker Server

3. Consumer（消费者）


## `topic`（消息的类别）
1. 区分、隔离不同的消息数据，屏蔽了底层复杂的存储方式，开发的时候只需要关注数据写入到了哪个`topic`、从哪个`topic`取出数据

2. `topic`是一个逻辑概念，`MQ`中的抽象概念，是一个消费标示，用于保证`Producer`以及`Consumer`能够通过该标示进行对接


## `partition`（数据存储的基本单元）
1. 同一个`topic`的数据，会被分散的存储到多个`partition`中

2. 这些`partition`可以在同一台机器上，也可以是在多台机器上

3. `partition`作为基本实现单元，消费时，每个消费线程最多只能使用一个`partition`，一个`topic`中`partition`的数量，就是每个`group`中消费该`topic`的最大并行度数量


## `consumer group`（消费组）
1. 同一个`topic`，每个`group`都可以拿到同样的所有数据

2. 同一个`group`，如果有一个`consumer`已经消费了一个`topic`中的数据，那么消费过的数据其它`consumer`再无法获取到（如果`ConsumerA`以及`ConsumerB`同在一个`group`，那么`ConsumerA`消费的数据`ConsumerB`就无法消费了）

3. `group`下可以有一个或多个`instance`，`instance`可以是一个进程，也可以是一个线程

4. `group.id`是一个字符串，唯一标识一个`group`，`topic`下的每个分区只能分配给某个`group`下的一个`consumer`(当然该分区还可以被分配给其他`group`)


## `consumer`（消费者）
1. `consumer`的数量通常不超过`partition`的数量，且二者最好保持整数倍关系，因为`Kafka`在设计时假定了一个`partition`只能被一个`consumer`消费（同一`group`内）。


## `broker`
1. 物理概念，指服务于`kafka`的一个`node`。


## `offset`
1. `offset`专指`partition`以及`consumer group`而言，记录某个`consumer group`在某个`partiton`中当前已经消费到达的位置


## `kafka`命令
1. 消费kafka
/usr/local/kafka/bin/kafka-console-consumer.sh --zookeeper kafka.in.netwa.cn:2181 --topic ImTopic --from-beginning

2. 列出kafka信息
/usr/local/kafka/bin/kafka-topics.sh --zookeeper kafka.in.netwa.cn:2182 --describe --topic ImTopic

3. 列出最小偏移
/usr/local/kafka/bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list kafka.in.netwa.cn:9092 -topic SysMsg --time -2

4. 列出最大偏移
/usr/local/kafka/bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list kafka.in.netwa.cn:9092 -topic SysMsg --time -1
/usr/local/kafka/bin/kafka-run-class.sh kafka.tools.GetOffsetShell --broker-list kafka.in.netwa.cn:9092 -topic ImTopic --time -1

5. 发消息
curl -XPOST 'http://127.0.0.1:8002/v1/userinfo/send_im_msg' -H 'user_id: 4bdfec58-3d63-4f33-84f5-b8c6bb5e5803' -d '{"Content":"发送用户信息"}'
























#查看topic列表
bin/kafka-topics.sh --list --zookeeper 127.0.0.1:2181

#创建topic：
bin/kafka-topics.sh --create --zookeeper 127.0.0.1:2181 --replication-factor 1 --partitions 5 --topic test

#查看"test"主题详情
bin/kafka-topics.sh --describe --zookeeper  127.0.0.1:2181 --topic test

#为主题test增加2个分区数量
bin/kafka-topics.sh --alter --zookeeper localhost:2182 --topic test --partitions 2

#启动生产者
bin/kafka-console-producer.sh --broker-list 127.0.0.1:9092 --topic test

#启动消费者
#  --from-beginning代表启动后将这个主题之前的消息都显示出来。
bin/kafka-console-consumer.sh --zookeeper 127.0.0.1:2181 --topic test --from-beginning

#查看消费者组列表
bin/kafka-consumer-groups.sh --bootstrap-server 127.0.0.1:9092 --list

#查看某个消费者组的偏移量(offset)
bin/kafka-consumer-groups.sh --bootstrap-server 192.168.33.69:9092 --describe --group my-group






kafka-console-consumer.sh --zookeeper 192.168.0.152:2181 --topic topic_user --from-beginning

kafka-console-producer.sh --broker-list 192.168.0.152:9092 --topic topic_user





