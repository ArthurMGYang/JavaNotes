## MySQL

### 存储引擎

MySQL当前默认引擎是InnoDB，而在5.5之前默认为MyISAM。二者对比如下

1. 行级锁。MyISAM只有表级锁，而InnoDB支持行级锁和表级锁，默认为行级锁
2. 事务和崩溃后安全恢复：MyISAM强调性能，每次查询具有原子性，其执行速度比InnoDB快，但不提供事务支持。InnoDB提供事务，外部键等高级数据库功能。
3. MyISAM不支持外键，而InnoDB支持
4. MVCC：只有InnoDB支持。

### 索引

MySQL索引使用的数据结构主要是BTree索引和哈希索引。

哈希索引底层是哈希表，因此绝大多数需求为单挑记录查询的时候，哈希索引最快

其他情况下用BTree。BTree是B树中的B+Tree，不同的引擎实现方式不同。

1. MyISAM：B+Tree的叶节点的data域存放的是数据记录的地址。索引时，首先按照搜索算法搜索索引，如果Key存在，取出data域值，然后以data域的值为地址读取相应数据，这叫做“非聚簇索引”

2. InnoDB：其数据本身就是索引文件。树的叶节点data域保存了完整的数据记录，这个索引的key是数据表的主键，因此InnoDB表数据文件本身就是主索引，这叫做”聚簇索引“。

   在根据主索引搜索时，找到key所在的节点即可取出数据；在根据辅助索引搜索时，则需要先取出主键的值，再走一遍主索引。因此在设计表时，过长字段和非单调字段都不适合作为主键，这样会造成主索引频繁分裂。

### 事务

事务是逻辑上一组操作，要么全部执行，要么都不执行。

（经典例子：转账。一方扣钱，一方增加钱）

#### 四大特性

1. 原子性(Atomicity)：事务是最小的执行单位，不能分割；
2. 一致性(Consistency)：执行事务前后，数据保持一致，多个事务对同一个数据读取的结果是相同的；
3. 隔离性(Isolation)：并发访问数据库时，一个用户的事务不被其他事务所干扰，各并发事务之间数据库是独立的；
4. 持久性(Durability)：一个事务被提交之后，它对数据库中数据的改变是持久的，即使数据库发生故障也不应该对其有影响

#### 并发事务存在的问题

1. 脏读(Dirty read)：一个事务修改了数据还没上传而另一个事务获取了没修改的数据，那么这个没修改的数据就叫做脏数据，做出的相应操作是错误的
2. 丢失修改(Lost of Modify)：两个事务都读取了某个数据，第一个先完成，第二个后完成，并且都进行了修改，那么第一个事务的修改就会丢失
3. 不可重复读(Unrepeatable read)：指一个事务内多次读同一数据，而在这个事务两次读数据之间，有另一个事务修改了数据，这就造成了一个事务内两次读到的数据不一样的情况
4. 幻读(Phantom read)：与不可重复读类似。但是这个是在第一个事务两次读取间，第二个事务插入了一些数据，使得第一个事务会发现多了许多不存在的记录。

#### 事务隔离级别

1. READ-UNCOMMITTED(读取未提交)：允许读取尚未提交的数据，可能出现脏读，幻读和不可重复读
2. READ-COMMITTED(读取已提交)：允许读取并发事务已经提交的数据，但是还是会发生不可重复读和幻读
3. REPEATABLE-READ(可重复度)：对同一字段的多次读取结果都是一致的，除非数据是被本身事务自己所修改，仍然会发生幻读
4. SERIALIZABLE(可串行化)：最高隔离级别，完全服从ACID隔离级别。所有的事物依次逐个执行，这样事务之间就完全不可能产生干扰，所有问题都不会发生。

InnoDB默认隔离级别是REPEATABLE-READ

### 锁机制与InnoDB锁算法

MyISAM：表级锁

InnoDB：行级锁和表级锁，默认行级锁

#### 行级锁和表级锁

**表级锁**：MySQL中锁定粒度最大的一种锁，对当前操作的整张表加锁，实现简单，资源消耗也比较少，加锁快，不会死锁。锁定粒度大，触发所冲突的概率高，并发度最低

**行级锁**：MySQL中锁定粒度最小的一种锁，只针对当前操作进行加锁。行级锁能大大减少数据库操作的冲突，并发度高，但是开销大，加锁慢，会出现死锁。

#### InnoDB锁算法

1. Record lock：单个行记录上锁
2. Gap lock：间隙锁，锁定一个范围，不包括记录本身
3. Next-key lock：锁定一个范围，包含记录本身

InnoDB针对行的查询使用next-key lock

Next-locking keying为了解决幻读问题

当查询的索引含有唯一属性时，next-key lock降级为record key

gap锁的设计目的是为了阻止多个事务将记录插入同一范围，解决幻读问题

关闭gap锁的方式：1. 将事务的隔离级别设置为RC 2. 将innodb-locks-unsafe-for-binlog设置为1

### 大表优化

当MySQL单表记录数过大时，数据库CRUD性能明显下降，常见优化如下：

1. **限定数据范围**：禁止不带任何限制数据范围的查询语句，例如查订单的时候限定范围一个月

2. **读/写分离**：经典的数据库拆分方案，主库负责写，从库负责读

3. **垂直分区**：根据数据库里面数据的相关性进程拆分。垂直拆分是指数据表列的拆分，把一张列比较多的表拆成多张表。例如用户表既有登录信息又有其他信息，可以拆成两个独立的表，甚至单独的库。

   优点：可以使列数据变小，查询时减少读取的Block数，较少I/O次数。垂直分区的简化表结构易于维护。

   缺点：主键冗余，需要管理冗余列，并且会引起join操作，可以通过在应用层进行join来解决。事务也会变得更复杂

4. **水平分区**：保持表结构不变，通过某种策略存储数据分片。这样每一片2数据分散到不同的表或者库中，达到了分布式的目的。水平拆分可以职称非常大的数据量。

### 池化思想与数据库连接池

池化思想：是一种设计，会初始预设资源，解决每次获取资源的消耗。

数据库连接池本质上是socket的链接。可以把数据库连接池看做是维护的数据库连接的缓存，以便将来需要对数据库的请求时可以重用这些连接，为每个用户打开和维护数据库连接，尤其是对动态数据库驱动的网站应用程序的请求，既昂贵又浪费资源。在连接池中，创建连接之后，将其放置在池中，并再次使用它，因此不必建立新的连接。如果使用了所有连接，则会建立一个新连接并将其添加到池中。

### 分库后的id主键处理

分库分表后，如果每个表都从1开始算ID，就会出错，所以需要一个全局唯一的id来支持。

* UUID：太长了不适合作为主键，并且无序不可读，查询效率低，比较适合用于生成类似文件名等
* 数据库自增id：两台数据库分别设置不同步长，生成不重复ID的策略来实现高可用。这种方式生成的ID有序，但是需要独立部署数据库实例
* Twitter的snowflake算法
* 美团的Leaf分布式ID生成系统