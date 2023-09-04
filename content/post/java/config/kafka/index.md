---
title: "kafka 基础配置"

description:

date: 2023-09-03T16:09:47+08:00

math:

license:

hidden: false

comments: true

draft: false

tags: ["kafka"]

categories: ["基线配置"]
---

## 软件版本

[kafka\_2.12-2.4.0.tgz（带zookeeper）](http://archive.apache.org/dist/kafka/2.4.0/kafka_2.12-2.4.0.tgz)


## 服务端部署
### 修改kafka配置文件 server.properties

**vim /data/kafka/config/server.properties:**

```
# Kafka broker的唯一标识符，每个broker在集群中必须有个唯一的ID。
broker.id=0

# 监听相关参数
1steners SASLPLAINTEXT://192.168.1.131:9092 
advertised.1fsteners=SASL_PLAINTEXT://192.168.1.131:9092 

# 主题相关参数
# 禁止自动创建主题
auto.create.topics.enable=false  
# 启用删除topic功能
delete.topic.enable=true

# 默认值1。当该副本所在的brker机，consumer_offsets 只有一份副本，该分区岩机。使用该分区存储消费分组offset位置的消费者均会收到影响，offset无法提交，从而导致生产者可以发送消息但消费者不可用。所以需要设置该字段的值大于1
offsets.topic.replication.factor=3

# 事务主题的复制因子(设置更高一确保可用性) 设置为节点数 内部主题创建将失败，直到集群大小满足此复制因素要求
transaction.state.log.replication.factor=3 

# 覆盖事务主题的min.insync.replicas配置。设置为(节点数+1)/2
transaction.state.log.min.isr=2

# Kafka ISR 列表中最小同步副本数。设置为(节点数+1)/2 
min.insync.replicas=2

# 事务最大超时时间默认60000ms
transaction.max.timeout.ms=900000
# 默认副本因子
default.replication.factor=3

# 线程相关参数
# 后台处理任务的线程数，如果不出问题，默认不修改默认10
background.threads=10
# 处理磁盘IO的线程数量 默认8
num.io.threads=8
# 处理网络请求的线程数量 默认3 
num.network.threads=
# 在启动时用于日志恢复和在关闭时刷新的每个数据目录的线程数默认1
num.recovery.threads.per.data.dir=1

# 用于复制来自源代理的消息线程的数量 默认1 增加该值可以增加 follower broker 中的 I/O 平行度的程度。 
queued.max.requests=500 

# 默认值500 用于控制生产者在发送请求的时候可以排队等待的最大请求数量 如果生产者发送请求的速度超过了服务器处理请求的速度，未发送的请求将积压在生产者的队列中 
queued.max.requests=500

# 发送套接字的缓冲区大小 
socket.send.buffer.bytes=104857600 
# 接受套接字的缓冲区大小
socket.receive.buffer.bytes=104857600 
# 请求套接字的缓冲区大小
socket.request.max.bytes=104857600


#压缩相关参数 默认producer 意为和producer端的压缩格式保持一致 
compression.type=producer

# zookeeper相关参数
zookeeper.connection.timeout.ms=6000
zockeeper.sync.time.ms=2000 zookeeper.connect=192.168.1.112:2181,192.168.1.111:2181,192.168.1.110:2181/kafka

# 重平衡相关参数
# Unclean Leader Election指在Kafka集群leader节点失败时,新选举出来的leader节点可能还没有获得全部partition的最新offset数据,这时候新leader就是一个“脏”leader。
# 正常情况下,Kafka会等待新leader从其他副本那里同步完毕数据后,才会开始作为leader提供服务。
# 但开启脏选举功能后,即使数据同步未完成,新选举出来的leader也会直接开始提供服务,这可能会导致一部分数据丢失。
unclean.leader.election.enable=false 

# 禁止自动重平衡
auto.leader.rebalance.enable=false

# 检查各个分区是否平衡的频率 默认300s
leader.imbalance.check.interval.seconds=30 

# 触发重平衡的阈值百分比 默认为10
group.initial.rebalance.delay.ms=10000

# 日志刷写相关参数 暂不更改

log.dirs=/applog/kafka/logs

# segment文件保备的最长时间，超时将被删除 默认7天168 
log.retention.hours=168

# 日志滚动切片相关参数
# 日志滚动的最大时长，默认7天
log.roll.hours=48

# 给日志段的切分加一个扰动值，默认为0 避免大量日志段在同一时间进行切分操作 如果发现kafka有周期性的磁盘I/O打满情况，建议设置此值。
log.roll.jitter.hours=2

# 元数据相关参数

# 副本相关参数暂不更改
# 默认值500 follower副本发出的每个获取器请求的最大等待时间。该值在任何时候都应该小于replica.lag.time.max.ms，以防止低吞吐量主题的ISR频繁收缩 
replica.fetch.wait.max.ms=500
# 默认值5000 将高水位保存到磁盘的频率
replica.high.watermark.checkpoint.interval.ms=5000
# 默认值30000 如果follower没有发送任何fetch请求，或者没有消耗leader将从ISR中删除follower 
replica.log.time.max.ms=30000
# 默认值65536 用于网络请求的套接字接收缓冲区 
replica.socket.receive.buffer.bvtes=65536

# 网络请求的套接字超时。它的值至少应该是 replica.fetch.wait.max.ms 
replica.socket.timeout.ms=30000
# 定义副本节点与领导者leader节点之间的最大复制滞后时间表示在副本节点从领导者节点获取数据之前可以容忍的最大时间延迟 
repiica.1aq.time.max.ms=10000

# 消息相关参数
message.max.bytes=10485760
replica.fetch.max.bvtes=10485760
log.message.timestamp.type=CreateTime

# 默认1000 这个参数暂定 较小的值可以提高请求处理效率但增大清理操作的开销 
fetch.purgatory.purge.interval.requests=100

# 权限配置
allow.everyone.if.no.acl.found=true
authorizer.class.name=kafka.security.auth.SimpleAclAuthorizer
security.inter.broker.protocol=SASL_PLAINTEXT
sasl.mechanism.inter.broker.protocol=SCRAM-SHA-256
sasl.enabled.mechanisms=SCRAM-SHA-256
super.users=User:admin

```

### 添加SASL配置文件
在配置目录下创建一个 kafka_server_jaas.conf 文件  
** vim kafka_server_jaas.conf: **

```
KafkaServer {
  org.apache.kafka.common.security.scram.ScramLoginModule required
  username="admin"
  password="admin"
  user_admin="admin"
  user_producer="pwd1"
  user_consumer="pwd2";
};


```

基本语法注意事项：
第一行：固定名称指定 KafkaServer 端的配置。
第二行：安全认证类名为 ScramLoginModule，与 Kafka 属性文件中的协议类型一致。
第三、四行：服务端使用的帐号和密码。
第五行，超管帐号的密码。
后面都是预设普通帐号认证信息，语法为 user_真正的用户名=''密码"
最后一行用户名密码定义后，必须以分号 ; 结尾。

### 2.4 修改启动脚本

vim bin/kafka-server-start.sh ，找到 **export KAFKA_HEAP_OPTS** , 添加jvm 文件：

```
export KAFKA_HEAP_OPTS="-Xmx6G -Xms6G -Djava.security.auth.login.config=/approot/kafka/config/kafka_server_jaas.conf"
```

### 2.5 启动kafka服务
向 Zookeeper 注册 kafka_server_jaas.conf 文件中定义的各个用户的密码
```shell
bin/kafka-configs.sh --zookeeper localhost:2181 --alter --add-config 'SCRAM-SHA-256=[password=admin]' --entity-type users --entity-name admin

```
这一步必须配置，否则启动 Kafka 的时候会报错：

```
/approot/kafka/bin/kafka-server-start.sh -daemon /approot/kafka/config/server.properties 
```

## kafka客户端配置登录认证
### 创建客户端认证配置文件

在配置目录下创建一个 producer.conf 文件：  

```
security.protocol=SASL_PLAINTEXT
sasl.mechanism=SCRAM-SHA-256
sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required username="producer" password="pwd1";
```

测试命令
```shell
bin/kafka-console-producer.sh --broker-list localhost:9092 --topic test  --producer.config /approot/kafka/config/producer.conf
```

在配置目录下创建一个 consumer.conf 文件：  
```shell
security.protocol=SASL_PLAINTEXT
sasl.mechanism=SCRAM-SHA-256
sasl.jaas.config=org.apache.kafka.common.security.scram.ScramLoginModule required username="consumer" password="pwd2";
```
