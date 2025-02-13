# Kafka学习

-------

## **一、Kafka 简介**

**Kafka** 是一个消息系统，原本开发自 LinkedIn，用作 LinkedIn 的活动流（Activity Stream）和运营数据处理管道（Pipeline）的基础。现在它已被[多家不同类型的公司](https://link.zhihu.com/?target=https%3A//cwiki.apache.org/confluence/display/KAFKA/Powered%2BBy) 作为多种类型的数据管道和消息系统使用。

Kafka 是一种分布式的，基于发布 / 订阅的消息系统。主要设计目标如下：

- 以时间复杂度为 O(1) 的方式提供消息持久化能力，即使对 TB 级以上数据也能保证常数时间复杂度的访问性能。
- 高吞吐率。即使在非常廉价的商用机器上也能做到单机支持每秒 100K 条以上消息的传输。
- 支持 Kafka Server 间的消息分区，及分布式消费，同时保证每个 Partition 内的消息顺序传输。
- 同时支持离线数据处理和实时数据处理。
- Scale out：支持在线水平扩展。

假如你是一个程序员，假设你维护了两个服务 A 和 B。
B 服务每秒只能处理 100 个消息，但 A 服务却每秒发出 200 个消息，B 服务哪里顶得住，分分钟被压垮。
那么问题就来了，有没有办法让 B 在不被压垮的同时，还能处理掉 A 的消息？
当然有，**没有什么是加一层中间层不能解决的，如果有，那就再加一层**。

## 什么是消息队列

![消息队列在B进程里](https://cdn.xiaobaidebug.top/1713671959546.jpeg)

为了保护 B 服务，我们很容易想到可以在 B 服务的内存中加入一个**队列**。

其实就是个链表，链表的每一个节点就是一个消息，每个节点有一个序号，我们叫它Offset，记录消息1位置。B服务依据自己的处理能力，消费链表里面的消息，能处理多少是多少不断更新已处理 Offset 的值。但这有个问题，来不及处理的消息会堆积在内存里，如果 B 服务更新**重启**，这些消息就都丢了。
这个好解决，将队列挪出来，变成一个**单独的进程**。就算 B 服务重启，也不会影响到了队列里的消息。

![消息队列单独一个进程](https://cdn.xiaobaidebug.top/1713671978580.jpeg)

这样一个简陋的队列进程，其实就是所谓的**消息队列**。
而像 A 服务这样负责发数据到消息队列的角色，就是**生产者**，像 B 服务这样处理消息的角色，就是**消费者**。

![生产者和消费者](https://cdn.xiaobaidebug.top/1713672007372.jpeg)

但这个消息队列属实过于简陋，像什么高性能，高扩展性，高可用，它是一个都不沾。
我们来看下怎么优化它。

## 高性能

B 服务由于性能较差，消息队列里会不断堆积数据，为了提升性能，我们可以扩展更多的消费者, 这样消费速度就上去了，相对的我们就可以增加更多生产者，提升消息队列的吞吐量。

随着生产者和消费者都变多，我们会发现它们会同时争抢同一个消息队列，抢不到的一方就得等待，这不纯纯浪费时间吗！
有解决方案吗？有！

![多个topic](https://cdn.xiaobaidebug.top/1713672075754.jpeg)首先是对消息进行分类，每一类是一个 **topic**，然后根据 topic 新增队列的数量，生产者将数据按 topic 投递到不同的队列中，消费者则根据需要订阅不同的 topic。这就大大降低了 topic 队列的压力。

但单个 topic 的消息还是可能过多，我们可以将单个队列，拆成好几段，每段就是一个 **partition 分区**，每个消费者负责一个 partition。
这就大大降低了争抢，提升了消息队列的性能。

## 高扩展性

随着 partition 变多，如果 partition 都在同一台机器上的话，就会导致单机 cpu 和内存过高，影响整体系统性能。于是我们可以申请更多的机器，将 partition 分散部署在多台机器上，这每一台机器，就代表一个 **broker**。我们可以通过增加 broker 缓解机器 cpu 过高带来的性能问题。

## 高可用

到这里，其实还有个问题，如果其中一个 partition 所在的 broker 挂了，那 broker 里所有 partition 的消息就都没了。这高可用还从何谈起？
我们可以给 partition 多加几个副本，也就是 replicas，将它们分为 Leader 和 Follower。Leader 负责应付生产者和消费者的读写请求，而 Follower 只管同步 Leader 的消息。将 Leader 和 Follower 分散到不同的 broker 上，这样 Leader 所在的 broker 挂了，也不会影响到 Follower 所在的 broker, 并且还能从 Follower 中选举出一个新的 Leader partition 顶上。这样就保证了消息队列的高可用。

当我们讨论**可靠性**的时候，我们总会提到*保证**这个词语。可靠性保证是基础，我们基于这些基础之上构建我们的应用。比如关系型数据库的可靠性保证是ACID，也就是原子性（Atomicity）、一致性（Consistency）、隔离性（Isolation）和持久性（Durability）。

Kafka 中的可靠性保证有如下四点：

- 对于一个分区来说，它的消息是有序的。如果一个生产者向一个分区先写入消息A，然后写入消息B，那么消费者会先读取消息A再读取消息B。
- 当消息写入所有in-sync状态的副本后，消息才会认为**已提交（committed）**。这里的写入有可能只是写入到文件系统的缓存，不一定刷新到磁盘。生产者可以等待不同时机的确认，比如等待分区主副本写入即返回，后者等待所有in-sync状态副本写入才返回。
- 一旦消息已提交，那么只要有一个副本存活，数据不会丢失。
- 消费者只能读取到已提交的消息。

使用这些基础保证，我们构建一个可靠的系统，这时候需要考虑一个问题：究竟我们的应用需要多大程度的可靠性？可靠性不是无偿的，它与系统可用性、吞吐量、延迟和硬件价格息息相关，得此失彼。因此，我们往往需要做权衡，一味的追求可靠性并不实际。

## 持久化和过期策略

刚刚提到的是几个 broker 挂掉的情况，那搞大点，假设所有 broker 都挂了，那岂不是数据全丢了？
为了解决这个问题，我们不能光把数据放内存里，还要持久化到磁盘中，这样哪怕全部 broker 都挂了，数据也不会全丢，重启服务后，也能从磁盘里读出数据，继续工作。

但问题又来了，磁盘总是有限的，这一直往里写数据迟早有一天得炸。
所以我们还可以给数据加上保留策略，也就是所谓的 `retention policy`，比如磁盘数据超过一定大小或消息放置超过一定时间就会被清理掉。

## consumer group

到这里，这个消息队列好像就挺完美了。但其实还有个问题，按现在的消费方式，每次新增的消费者只能跟着**最新的**消费 Offset 接着消费。
如果我想让新增的消费者从某个 Offset 开始消费呢？
听起来这个需求很刁钻？我举个例子你就明白了。

哪怕 B 服务有多个实例，但本质上，它只有一个消费业务方，新增实例一般也是接着之前的 offset 继续消费。
假设现在来了个新的业务方，C 服务，它想从头开始消费消息队列里的数据，这时候就不能跟在 B 服务的 offset 后边继续消费了。

所以我们还可以给消息队列加入消费者组（consumer group）的概念，B 和 C 服务各自是一个独立的消费者组，不同消费者组维护自己的消费进度，互不打搅。

## ZooKeeper

相信你也发现了，组件太多了，而且每个组件都有自己的数据和状态，所以还需要有个组件去统一维护这些组件的状态信息，于是我们引入 **ZooKeeper** 组件。它会定期和 broker 通信，获取 整个 kafka 集群的状态，以此判断 某些 broker 是不是跪了，某些消费组消费到哪了。

## **二、Kafka 的设计与实现**

### **Kafka 存储在文件系统上**

Kafka 高度依赖文件系统来存储和缓存消息，一般的人认为 “磁盘是缓慢的”，所以对这样的设计持有怀疑态度。现代的操作系统针对磁盘的读写已经做了一些优化方案来加快磁盘的访问速度。比如，**预读**会提前将一个比较大的磁盘快读入内存。**后写**会将很多小的逻辑写操作合并起来组合成一个大的物理写操作。并且，操作系统还会将主内存剩余的所有空闲内存空间都用作**磁盘缓存**，所有的磁盘读写操作都会经过统一的磁盘缓存（除了直接 I/O 会绕过磁盘缓存）。综合这几点优化特点，**如果是针对磁盘的顺序访问，某些情况下它可能比随机的内存访问都要快，甚至可以和网络的速度相差无几。**

 **Topic 其实是逻辑上的概念，面相消费者和生产者，物理上存储的其实是 Partition**，每一个 Partition 最终对应一个目录，里面存储所有的消息和索引文件。默认情况下，每一个 Topic 在创建时如果不指定 Partition 数量时只会创建 1 个 Partition。比如，我创建了一个 Topic 名字为 test ，没有指定 Partition 的数量，那么会默认创建一个 test-0 的文件夹，这里的命名规则是：`<topic_name>-<partition_id>`。

任何发布到 Partition 的消息都会被追加到 Partition 数据文件的尾部，这样的顺序写磁盘操作让 Kafka 的效率非常高（经验证，顺序写磁盘效率比随机写内存还要高，这是 Kafka 高吞吐率的一个很重要的保证）。

每一条消息被发送到 Broker 中，会根据 Partition 规则选择被存储到哪一个 Partition。如果 Partition 规则设置的合理，所有消息可以均匀分布到不同的 Partition中。

### **Kafka 中的底层存储设计**

假设我们现在 Kafka 集群只有一个 Broker，我们创建 2 个 Topic 名称分别为：「topic1」和「topic2」，Partition 数量分别为 1、2，那么我们的根目录下就会创建如下三个文件夹：

```text
    | --topic1-0
    | --topic2-0
    | --topic2-1
```

在 Kafka 的文件存储中，同一个 Topic 下有多个不同的 Partition，每个 Partition 都为一个目录，而每一个目录又被平均分配成多个大小相等的 **Segment File** 中，Segment File 又由 index file 和 data file 组成，他们总是成对出现，后缀 ".index" 和 ".log" 分表表示 Segment 索引文件和数据文件。

现在假设我们设置每个 Segment 大小为 500 MB，并启动生产者向 topic1 中写入大量数据，topic1-0 文件夹中就会产生类似如下的一些文件：

```text
    | --topic1-0
        | --00000000000000000000.index
        | --00000000000000000000.log
        | --00000000000000368769.index
        | --00000000000000368769.log
        | --00000000000000737337.index
        | --00000000000000737337.log
        | --00000000000001105814.index
        | --00000000000001105814.log
    | --topic2-0
    | --topic2-1
```

**Segment 是 Kafka 文件存储的最小单位。**Segment 文件命名规则：Partition 全局的第一个 Segment 从 0 开始，后续每个 Segment 文件名为上一个 Segment 文件最后一条消息的 offset 值。数值最大为 64 位 long 大小，19 位数字字符长度，没有数字用0填充。如 00000000000000368769.index 和 00000000000000368769.log。

其中以索引文件中元数据 `<3, 497>` 为例，依次在数据文件中表示第 3 个 message（在全局 Partition 表示第 368769 + 3 = 368772 个 message）以及该消息的物理偏移地址为 497。

注意该 index 文件并不是从0开始，也不是每次递增1的，这是因为 Kafka 采取稀疏索引存储的方式，每隔一定字节的数据建立一条索引，它减少了索引文件大小，使得能够把 index 映射到内存，降低了查询时的磁盘 IO 开销，同时也并没有给查询带来太多的时间消耗。

因为其文件名为上一个 Segment 最后一条消息的 offset ，所以当需要查找一个指定 offset 的 message 时，通过在所有 segment 的文件名中进行二分查找就能找到它归属的 segment ，再在其 index 文件中找到其对应到文件上的物理位置，就能拿出该 message 。

由于消息在 Partition 的 Segment 数据文件中是顺序读写的，且消息消费后不会删除（删除策略是针对过期的 Segment 文件），这种顺序磁盘 IO 存储设计师 Kafka 高性能很重要的原因。

> Kafka 是如何准确的知道 message 的偏移的呢？这是因为在 Kafka 定义了标准的数据存储结构，在 Partition 中的每一条 message 都包含了以下三个属性：

- offset：表示 message 在当前 Partition 中的偏移量，是一个逻辑上的值，唯一确定了 Partition 中的一条 message，可以简单的认为是一个 id；
- MessageSize：表示 message 内容 data 的大小；
- data：message 的具体内容

### **消费组与分区重平衡**

当新的消费者加入消费组，它会消费一个或多个分区，而这些分区之前是由其他消费者负责的；另外，当消费者离开消费组（比如重启、宕机等）时，它所消费的分区会分配给其他分区。这种现象称为**重平衡（rebalance）**。重平衡是 Kafka 一个很重要的性质，这个性质保证了高可用和水平扩展。**不过也需要注意到，在重平衡期间，所有消费者都不能消费消息，因此会造成整个消费组短暂的不可用。**而且，将分区进行重平衡也会导致原来的消费者状态过期，从而导致消费者需要重新更新状态，这段期间也会降低消费性能。后面我们会讨论如何安全的进行重平衡以及如何尽可能避免。

消费者通过定期发送心跳（hearbeat）到一个作为组协调者（group coordinator）的 broker 来保持在消费组内存活。这个 broker 不是固定的，每个消费组都可能不同。当消费者拉取消息或者提交时，便会发送心跳。

如果消费者超过一定时间没有发送心跳，那么它的会话（session）就会过期，组协调者会认为该消费者已经宕机，然后触发重平衡。可以看到，从消费者宕机到会话过期是有一定时间的，这段时间内该消费者的分区都不能进行消息消费；

在 0.10.1 版本，Kafka 对心跳机制进行了修改，将发送心跳与拉取消息进行分离，这样使得发送心跳的频率不受拉取的频率影响。另外更高版本的 Kafka 支持配置一个消费者多长时间不拉取消息但仍然保持存活，这个配置可以避免活锁（livelock）。活锁，是指应用没有故障但是由于某些原因不能进一步消费。