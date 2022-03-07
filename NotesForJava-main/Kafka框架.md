## Kafka是什么以及应用场景

Kafka是一个分布式流式处理平台。

流平台三个关键功能：

1. **消息队列**：发布和订阅消息流。
2. **容错的持久方式存储记录消息流**：Kafka会把消息持久化到磁盘，有效避免了消息丢失的风险。
3. **流式处理平台**：在消息发布的时候进行处理，Kafka提供了一个完成的流式处理类库

两大应用场景：

1. 消息队列：建立实时流数据管道，以可靠地在系统或应用程序之间获取数据
2. 数据处理：构建实时的流数据处理程序来转换或处理数据流

### Kafka相对于其他消息队列的优势

除了Kafka还有RocketMQ，RabbitMQ，而Kafka的优势在于：

1. 极致的性能：基于Scala和Java语言，设计中大量使用了批量处理和异步的思想，最高可以每秒处理千万级别的消息
2. 生态系统兼容性：Kafka与周边生态系统的兼容性是最好的没有之一，尤其在大数据和流计算领域。

## 队列模型以及Kafka的消息模型

### 队列模型：早期的消息模型

使用队列作为消息通信载体，满足生产者与消费者模式，一条消息只能被一个消息者使用，未被消费的消息在队列中保留直到被消费或超时。

存在的问题：

假设一种情况：需要将生产者产生的消息分发给多个消费者，并且每个消费者都能接收到完成的消息内容。这种情况下，队列模型就没用了，虽然可以采用为每个消费者创建一个单独的队列，让生产者发送多份，但是这种方法不仅浪费资源，还违背了消息队列的目的。

### 发布-订阅模型：Kafka消息模型

发布订阅(Pub-Sub)模型使用主题(Topic)作为消息通信载体，类似于广播模式。

发布者发布一条消息，该消息通过主题传递给所有的订阅者，在一条消息广播之后才订阅的用户则收不到该条消息。

在发布-订阅模型中，如果只有一个订阅者，就和队列模型基本一样了，所以发布-订阅模型在功能层面上是可以兼容队列模型的

## Producer、Consumer、Broker、Topic、Partition

Producer：产生消息的一方

Consumer：消费消息的一方

Broker：代理。可以看作是一个独立的Kafka实例，多个Kafka Broker组成一个Kafka Cluster。

每个Broker中又包含了Topic以及Partition这两个概念

Topic：Producer将消息发送到特定的主题，Consumer通过订阅特定的Topic来消费消息

Partition：分区。属于Topic的一部分，一个Topic有多个Partition，并且同一个Topic下的Partition可以分布在不同的Broker上，这也就表明了一个Topic可以横跨多个Broker。

![image-20220112131134227](/Users/yangmao/Library/Application Support/typora-user-images/image-20220112131134227.png)

## Kafka多副本机制

Kafka为分区引入了多副本（Replica）机制。

分区中的多个副本之间会有一个叫做leader，其他的称为follower。发送的消息会被发送到leader副本，然后follower副本才能从leader副本中拉取消息进行同步。

生产者和消费者只与leader副本交互，可以理解为其他副本都是leader副本的拷贝，它们的存在只是为了保证消息存储的安全性。当leader副本发生故障时，会从follower副本中选举出一个新的leader，但是follower中如果有和leader同步程度达不到要求就无法参与leader的竞选。

好处：

1. Kafka通过给特定的Topic制定多个Partition，而各个Partition可以分布在不同的Broker上，这样便能提供比较好的并发能力（负载均衡）
2. Partition可以制定对应的Replica数，这也极大地提高了消息存储的安全性，提高了容灾能力，不过相应的增加了所需要的存储空间。

## Zookeeper在Kafka中的作用

Zookeeper主要是为Kafka提供元数据的管理功能。

（搭建一个Kafka环境并进Zookeeper看哪些文件和Kafka有关就知道其作用）

1. Broker注册
2. Topic注册
3. 负载均衡

## Kafka如何保证消息的顺序消费

某些情况下需要消息有严格的顺序，例如：更改会员等级，根据等级计算价格

在Kafka中，Partition是真正保存消息的地方，发送的消息都在这里。而分区又存在于Topic中，并且给特定的Topic制定了多个Partition。

每次添加消息到Partition时都会采用尾加法，所以只能保证分区中的消息有序而不能保证Topic中的Partition有序。消息被追加的时候，Partition会计算偏移量，Kafka通过偏移量保证消息在分区内的顺序。

如何保证Topic中的Partition有序。

1. 一个Topic只对应一个Partition。有效但是完全违背了Kafka的设计初衷以及优势
2. 发送消息时指定key/Partition。Kafka发送消息时，可以指定Topic，Partition，key，data4个参数。指定key或者Partition的话，只会发送到同一个Partition中，可以用表/对象的id做key

## Kafka如何保证消息不丢失

### 生产者丢失消息的情况

生产者调用send之后，消息可能因为网络问题并没有发送出去。

所以不能默认在调用send方法之后消息发送成功。为了确定发送成功，需要判断发送的结果。有两种办法可以判断

1. 使用消费者的get方法
2. 采用添加回调函数的方法

### 消费者丢失消息的情况

消息追加到Partition的时候会计算偏移量，这个偏移量除了可以确保消息的顺序，还能让消费者找到对应消息的位置。

当消费者拉取到分区的某个消息之后，消费者会自动提交offset。但是存在消费者提交的时候，本身挂了，消息没有被消费，但是偏移量已经提交了。

为了避免这种情况，可以关闭自动提交偏移量，每次真正的消费完消息之后再手动提交偏移量。

### Kafka弄丢消息

Kafka为分区引入了多副本机制。

即使有leader和follower机制，但是存在一种可能，leader所在的Broker挂掉，但是此时leader的一部分数据还没有被任何follower同步，就会造成数据丢失。

解决方法

1. 设置acks=all。acks的默认值是1，代表我们的消息被leader副本接收之后就算成功发送；而all表示所有副本都要成功接收消息，该消息才算成功发送。
2. 设置replication.factor>=3。这样可以保证每个分区至少又3个副本，虽然造成了数据冗余但是带来了安全性
3. 设置min.insync.replicas>1。表示至少要被写入到2个副本才算成功发送，默认值是1。但是为了保证服务器的高可用性，需要保证replication.factor>min.insync.replicas
4. 设置unclean.leader.election.enable=false。这表示leader发生故障的时候，就不从follower中和leader同步程度达不到要求的副本选leader，降低了消息丢失的可能性