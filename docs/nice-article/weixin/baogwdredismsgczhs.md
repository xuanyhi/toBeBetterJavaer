---
title: 刨根问底 Redis， 面试过程真好使
shortTitle: 刨根问底 Redis， 面试过程真好使
author: 菜农
category:
  - 微信公众号
---

大家好，二哥呀，欢迎来到我的频道。

Redis，是二哥一直提到的 Java 后端开发四大件之一，重要程度就不用我再过分强调了，本篇将通过 30 个问题，由浅入深，最大程度上覆盖整个Redis的问答内容。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-94ba953d-4c73-467b-abdc-e58e6d7b81c1.jpg)

在 Web 应用发展的初期阶段，一个网站的访问量本身就不是很高，直接使用关系型数据库就可以应付绝大部分场景。但是随着互联网时代的崛起，人们对于网站访问速度有着越来越高的要求，直接使用关系型数据库的方案在性能上就出现了瓶颈。因此在客户端与数据层之间就需要一个缓存层来分担请求压力，而 Redis 作为一款优秀的缓存中间件，在企业级架构中占有重要的地位，因此 Redis 也作为面试的必问项。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-5f5c4283-29f1-447c-a857-eeca198cf9e0.jpg)


* * *

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-1560b389-1d8d-42d1-9661-a0a5c8597f53.jpg)

Redis（Remote Dictionary Server）是一个开源的、键值对型的数据存储系统。使用C语言编写，遵守BSD协议，可基于内存也可持久化的日志型数据库，提供了多种语言的API，被广泛用于数据库、缓存和消息中间件。并且支持多种类型的数据结构，用于应对各种不同场景。可以存储多种不同类型值之间的映射，支持事务，持久化，LUA 脚本以及多种集群方案等。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-66e38efb-5246-48ae-86f4-936b7994b189.jpg)

**优点：**

1.  完全基于内存操作，性能极高，读写速度快，Redis 能够支持超过 100KB/s 的读写速率
2.  支持高并发，支持10万级别的并发读写
3.  支持主从模式，支持读写分离与分布式
4.  具有丰富的数据类型与丰富的特性（发布订阅模式）
5.  支持持久化操作，不会丢失数据

**缺点：**

1.  数据库容量受到物理内存的限制，不能实现海量数据的高性能读写
2.  相比关系型数据库，不支持复杂逻辑查询，且存储结构相对简单
3.  虽然提供持久化能力，但实际更多是一个 disk-backed 功能，与传统意义上的持久化有所区别

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-60c426d4-f390-4f9f-b5b5-5f65e21df2f0.jpg)

> Memcache 也是一个开源、高性能、分布式内存对象缓存系统。所有数据均存储在内存中，在服务器重启之后就会消失，需要重新加载数据，采用 hash 表的方式将所有数据缓存在内存中，采用 LRU 算法来逐渐把过期的数据清除掉。

1.  **数据类型**：Memcache 仅支持字符串类型，Redis 支持 5 种不同的数据类型
2.  **数据持久化**：Memcache 不支持持久化，Redis 支持两种持久化策略，RDB 快照 和 AOF 日志
3.  **分布式**：Memcache 不支持分布式，只能在客户端使用一致性哈希的方式来实现分布式存储，Redis3.0 之后可在服务端构建分布式存储，Redis集群没有中心节点，各个节点地位平等，具有线性可伸缩的功能。
4.  **内存管理机制**：Memcache数据量不能超出系统内存，但可以调整内存大小，淘汰策略采用LRU算法。Redis增加了 VM 特性，实现了物理内存的限制，它们之间底层实现方式以及客户端之间通信的应用协议不一样。
5.  **数据大小限制**：Memcache 单个 key-value 大小有限制，一个Value最大容量为 1MB，Redis 最大容量为512 MB

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-390ab912-3fd4-4849-a9d4-00c3b451769c.jpg)

**基本数据类型：**

*   String（字符串）
*   Hash（哈希）
*   List（列表）
*   Set（集合）
*   ZSet（Sorted Set 有序集合）

**高级数据类型：**

*   HyperLogLog：用来做基数统计的算法，在输入元素的数量或体积非常大时，计算基数所需的空间总是固定的，并且是很小的。HyperLogLog 只会根据输入元素来计算基数，而不会存储输入元素本身
*   Geo：用来地理位置的存储和计算
*   BitMap：实际上不是特殊的存储结构，本质上是二进制字符串，可以进行位操作，常用于统计日活跃用户等

> *扩展：* geohash通过算法将1个定位的经度和纬度2个数值，转换成1个hash字符串。如果2个地方距离越近，那么他们的hash值的前缀越相同。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-3476164a-ac24-41b6-8f74-a8835d8c4dc5.jpg)

Redis 底层实现了简单动态字符串的类型（Simple Dynamic String，SDS）来表示 String 类型。没有直接使用C语言定义的字符串类型。

> SDS 实现相对于C语言String方式的提升

1.  避免缓冲区移除。对字符修改时，可以根据 len 属性检查空间是否满足要求
2.  获取字符串长度的复杂度较低
3.  减少内存分配次数
4.  兼容C字符串函数，可以重用C语言库的一部分函数

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-7d81ee1c-f7bb-4cb7-ad26-768f5bbe5d26.jpg)

Redis 直接以内存的方式存储可以达到最快的读写速度，如果开启了持久化则通过异步的方式将数据写入磁盘，因此Redis 具有快速和数据持久化的特征。

在内存中操作本身就比从磁盘操作更快，且不受磁盘I/O速度的影响。如果不将数据放在内存中而是保存到磁盘，磁盘I/O速度会严重影响到Redis 的性能，而数据集大小如果达到了内存的最大限定值则不能继续插入新值。

如果打开了虚拟内存功能，当内存用尽时，Redis就会把那些不经常使用的数据存储到磁盘，如果Redis中的虚拟内存被禁了，它就会操作系统的虚拟内存（交换内存），但这时Redis的性能会急剧下降。如果配置了淘汰机制，会根据已配置的数据淘汰机制来淘汰旧数据。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-dc9f5773-624d-407b-8902-09291d6613fe.jpg)

1、**尽可能使用哈希表（hash 数据结构）**：Redis 在储存小于100个字段的Hash结构上，其存储效率是非常高的。所以在不需要集合（set）操作或 list 的push/pop 操作的时候，尽可能使用 hash 结构。

2、**根据业务场景，考虑使用 BitMap**

3、**充分利用共享对象池**：Redis 启动时会自动创建【0-9999】的整数对象池，对于 0-9999的内部整数类型的元素，整数值对象都会直接引用整数对象池中的对象，因此尽量使用 0-9999 整数对象可节省内存。

4、**合理使用内存回收策略**：过期数据清除、expire 设置数据过期时间等

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-557f8beb-a419-4b1f-9c4f-3f8eed949540.jpg)

> Redis 能够用来实现分布式锁的命令有 INCR、SETNX、SET，并利用过期时间命令 expire 作为辅助

**方式1：利用 INCR**

如果 key 不存在，则初始化值为 0，然后再利用 `INCR` 进行加 1 操作。后续用户如果获取到的值大于等于 1，说明已经被其他线程加锁。当持有锁的用户在执行完任务后，利用 `DECR` 命令将 key 的值减 1，则表示释放锁。

**方式2：利用 SETNX**

先使用 `setnx` 来争抢锁，抢到之后利用 `expire` 设置一个过期时间防止未能释放锁。`setnx` 的意义是如果 key 不存在，则将key设置为 value，返回 1。如果已存在，则不做任何操作，返回 0。

**方式3：利用 SET**

`set` 指令有非常复杂的参数，相当于合成了 `setnx` 和 `expire` 两条命令的功能。其命令格式如：`set($Key,$value, array('nx', 'ex'=>$ttl))`。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-c8cdee88-29ad-4a06-b441-befe13172ce5.jpg)

1.  完全基于内存
2.  数据结构简单，操作方便，并且不同数据结构能够应对于不同场景
3.  采用单线程（网络请求模块使用单线程，其他模块仍用了多线程），避免了不必要的上下文切换和竞争条件，也不存在多进程或多线程切换导致CPU消耗，不需要考虑各种锁的问题。
4.  使用多路I/O复用模型，为非阻塞I/O
5.  Redis 本身设定了 VM 机制，没有使用 OS 的Swap，可以实现冷热数据分离，避免因为内存不足而造成访问速度下降的问题

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-c88f1351-0ecc-4faf-abd5-c2ecf6f8e10c.jpg)

**1、RDB（Redis DataBase）持久化**

RDB 是 Redis 中默认的持久化机制，按照一定的时间将内存中的数据以快照的方式保存到磁盘中，它会产生一个特殊类型的文件 `.rdb` 文件，同时可以通过配置文件中的 `save` 参数来定义快照的周期

在 RDB 中有两个核心概念 `fork` 和 `cow`，在执行备份的流程如下：

在执行`bgsave`的时候，Redis 会 fork 主进程得到一个新的子进程，子进程是共享主进程内存数据的，会将数据写到磁盘上的一个临时的 `.rdb` 文件中，当子进程写完临时文件后，会将原来的 `.rdb` 文件替换掉，这个就是 fork 的概念。那 cow 全称是 copy-on-write ，当主进程执行读操作的时候是访问共享内存的，而主进程执行写操作的时候，则会拷贝一份数据，执行写操作。

***优点***

1.  只有一个文件 dump.rdb ，方便持久化
2.  容错性好，一个文件可以保存到安全的磁盘
3.  实现了性能最大化，fork 单独子进程来完成持久化，让主进程继续处理命令，主进程不进行任何 I/O 操作，从而保证了Redis的高性能
4.  RDB 是一个紧凑压缩的二进制文化，RDB重启时的加载效率比AOF持久化更高，在数据量大时更明显

***缺点***

1.  可能出现数据丢失，在两次RDB持久化的时间间隔中，如果出现宕机，则会丢失这段时间中的数据
2.  由于RDB是通过fork子进程来协助完成数据持久化，如果当数据集较大时，可能会导致整个服务器间歇性暂停服务

**2、AOF（Append Only File）持久化**

AOF 全称是 Append Only File（追加文件）。当 Redis 处理每一个写命令都会记录在 AOF 文件中，可以看做是命令日志文件。该方式需要设置 AOF 的同步选项，因为对文件进行写入并不会马上将内容同步到磁盘上，而是先存储到缓冲区中，同步选项有三种配置项选择：

*   `always`：同步刷盘，可靠性高，但性能影响较大
*   `everysec`：每秒刷盘，性能适中，最多丢失 1 秒的数据
*   `no`：操作系统控制，性能最好，可靠性最差

为了解决 AOF 文件体检不断增大的问题，用户可以向 Redis 发送 `bgrewriteaof` 命令，可以将 AOF 文件进行压缩，也可以选择自动触发，在配置文件中配置

```
auto-aof-rewrite-precentage 100
auto-aof-rewrite-min-zise 64mb
```

***优点***

1.  实现持久化，数据安全，AOF持久化可以配置 appendfsync 属性为 always，每进行一次命令操作就记录到AOF文件中一次，数据最多丢失一次
2.  通过 append 模式写文件，即使中途服务器宕机，可以通过 Redis-check-aof 工具解决数据一致性问题
3.  AOF 机制的 rewrite 模式。AOF 文件的文件大小触碰到临界点时，rewrite 模式会被运行，重写内存中的所有数据，从而缩小文件体积

***缺点***

1.  AOF 文件大，通常比 RDB 文件大很多
2.  比 RDB 持久化启动效率低，数据集大的时候较为明显
3.  AOF 文件体积可能迅速变大，需要定期执行重写操作来降低文件体积

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-055fb9fa-5453-4969-b819-84ef9983b364.jpg)

**方式1：定时删除**

在设置 Key 的过期时间的同时，会创建一个定时器 timer，定时器在 Key 过期时间来临时，会立即执行对 Key 的删除操作

*特点：* 对内存友好，对 CPU 不友好。存在较多过期键时，利用定时器删除过期键会占用相当一部分CPU

**方式2：惰性删除**

key 不使用时不管 key 过不过期，只会在每次使用的时候再检查 Key 是否过期，如果过期的话就会删除该 Key。

*特点：* 对 CPU 友好，对内存不友好。不会花费额外的CPU资源来检测Key是否过期，但如果存在较多未使用且过期的Key时，所占用的内存就不会得到释放

**方式3：定期删除**

每隔一段时间就会对数据库进行一次检查，删除里面的过期Key，而检查多少个数据库，则由算法决定

*特点：* 定期删除是对上面两种过期策略的折中，也就是对内存友好和CPU友好的折中方法。每隔一段时间执行一次删除过期键任务，并通过限制操作的时长和频率来减少对CPU时间的占用。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-0933ed13-f214-4102-b5d0-870a6d33abb9.jpg)

> Redis 主从同步分为增量同步和全量同步Redis 会先尝试进行增量同步，如果不成功则会进行全量同步。

**增量同步：**

Slave 初始化后开始正常工作时主服务器发生的写操作同步到从服务器的过程。增量同步的过程主要是主服务器每执行一个写命令就会向从服务器发送相同的写命令。

**全量同步：**

Slave 初始化时它会发送一个 `psync` 命令到主服务器，如果是第一次同步，主服务器会做一次`bgsave`，并同时将后续的修改操作记录到内存 buffer 中，待 `bgsave` 完成后再将 RDB 文件全量同步到从服务器，从服务器接收完成后会将 RDB 快照加载到内存然后写入到本地磁盘，处理完成后，再通知主服务器将期间修改的操作记录同步到复制节点进行重放就完成了整个全量同步过程。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-a58be6cb-9a16-42d9-9766-65d144cf4582.jpg)

在Redis中，最大使用内存大小由Redis.conf中的参数maxmemory决定，默认值为0，表示不限制，这时实际相当于当前系统的内存。但如果随着数据的增加，如果对内存中的数据没有管理机制，那么数据集大小达到或超过最大内存的大小时，则会造成Redis崩溃。因此需要内存数据淘汰机制。

**设有过期时间**

1.  `volatile-lru`：尝试回收最少使用的键
2.  `volatile-random`：回收随机的键
3.  `volatile-ttl`：优先回收存活时间较短的键

**没有过期时间**

1.  `allkey-lru`：尝试回收最少使用的键
2.  `allkeys-random`：回收随机的键
3.  `noeviction`：当内存达到限制并且客户端尝试执行新增，会返回错误

> *淘汰策略的规则*
> 
> *   如果数据呈现幂律分布，也就是一部分数据访问频率高，一部分数据访问频率低，则使用 allKeys-lru
> *   如果数据呈现平等分布，也就是所有的数据访问频率大体相同，则使用 allKeys-random
> *   关于 lru 策略，Redis中并不会准确的删除所有键中最近最少使用的键，而是随机抽取5个键（个数由参数maxmemory-samples决定，默认值是5），删除这5个键中最近最少使用的键。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-0ebf4f4c-4907-4019-bf6d-2f81dd4fc005.jpg)

**问题1：缓存穿透**

> 缓存穿透是指缓存和数据库上都没有的数据，导致所有请求都落到数据库上，造成数据库短时间内承受大量的请求而导致宕机

*解决：*

1.  使用布隆过滤器：将查询的参数都存储到一个 bitmap 中，在查询缓存前，如果 bitmap 存在则进行底层缓存的数据查询，如果不存在则进行拦截，不再进行缓存的数据查询
2.  缓存空对象：如果数据库查询的为空，则依然把这个数据缓存并设置过期时间，当多次访问的时候可以直接返回结果，避免造成多次访问数据库，但要保证当数据库有数据时及时更新缓存。

**问题2：缓存击穿**

> 缓存击穿是指缓存中没有但数据库中有的数据（一般是缓存时间到期），就会导致所有请求都落到数据库上，造成数据库段时间内承受大量的请求而宕机

*解决：*

1.  设置热点数据永不过期
2.  可以使用互斥锁更新，保证同一进程中针对同一个数据不会并发请求到 DB，减小DB的压力
3.  使用随机退避方式，失效时随机 sleep 一个很短的时间，再次查询，如果失败再执行更新

**问题3：缓存雪崩**

> 缓存雪崩是指大量缓存同一时间内大面积失效，后面的请求都会落到数据库上，造成数据库段时间无法承受大量的请求而宕掉

*解决：*

1.  在缓存失效后，通过加锁或者队列来控制读数据库写缓存的线程数量。比如对某个Key只允许一个线程查询和写缓存，其他线程等待
2.  通过缓存 reload 机制，预先去更新缓存，在即将发生高并发访问前手动触发加载缓存
3.  对于不同的key设置不同的过期时间，让缓存失效的时间点尽量均匀，比如我们可以在原有的失效时间基础上增加一个随机值，比如1~5分钟随机，这样每一个缓存的过期时间的重复率就会降低。
4.  设置二级缓存，或者双缓存策略。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-5922d7de-de7c-4a85-bfab-6efd557747a5.jpg)

缓存降级，其实都应该是指服务降级。在访问量剧增、服务响应出现问题（如响应延迟或不响应）或非核心服务影响到核心流程的性能的情况下，仍然需要保证核心服务可用，尽管可能一些非主要服务不可用，这时就可以采取服务降级策略。

服务降级的最终目的是保证核心服务可用，即使是有损的。服务降级应当事先确定好降级方案，确定哪些服务是可以降级的，哪些服务是不可降级的。根据当前业务情况及流量对一些服务和页面有策略的降级，以此释放服务器资源以保证核心服务的正常运行。

降级往往会指定不同的级别，面临不同的异常等级执行不同的处理。根据服务方式：可以拒接服务，可以延迟服务，也可以随机提供服务。根据服务范围：可以暂时禁用某些功能或禁用某些功能模块。总之服务降级需要根据不同的业务需求采用不同的降级策略。主要的目的就是服务虽然有损但是总比没有好。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-094f1592-ec7e-42ef-9857-c2768bc51df6.jpg)

1.  数据实时同步失效或更新。这是一种增量主动型的方案，能保证数据强一致性，在数据库数据更新之后，主动请求缓存更新
2.  数据异步更新。这是一种增量被动型方案，数据一致性稍弱，数据更新会有延迟，更新数据库数据后，通过异步方式，用多线程方式或消息队列来实现更新
3.  定时任务更新。这是一种增/全量被动型方案，通过定时任务按一定频率调度更新，数据一致性最差

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-dbf8f783-af49-42a5-be7d-348dc8781e32.jpg)

1.  直接写个缓存刷新页面，上线时手工操作
2.  数据量不大，可以在项目启动时自动进行加载
3.  定时刷新缓存

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-a8eabf5b-7616-4974-b649-d89f7b953fa7.jpg)

> Sentinel（哨兵）适用于监控 Redis 集群中 Master 和 Slave 状态的工具，是Redis的高可用性解决方案

**主要作用**

1.  监控。哨兵会不断检查用户的Master和Slave是否运作正常
2.  提醒。当被监控的某个Redis节点出现问题时，哨兵可以通过API向管理员或其他应用程序发送通知
3.  自动故障迁移。当一个Master不能正常工作时，哨兵会开始一次自动故障迁移操作，它会将集群中一个Slave提升为新的Master，并让其他Slave改为与新的Master进行同步。当客户端试图连接失败的Master时，集群也会想客户端返回新的Master地址。当主从服务器切换后，新Master的Redis.conf，Slave的Redis.conf和Sentinel的Redis.conf三者配置文件都会发生相应的改变。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-4cbcb19c-d2cc-4a67-aad0-9a082d9996a7.jpg)

**问题背景**

> Redis 是基于TCP协议的请求/响应服务器，每次通信都要经过TCP协议的三次握手，所以当需要执行的命令足够复杂时，会产生很大的网络延迟，并且网络的传输时间成本和服务器开销没有计入其中，总的延迟可能更大。

**Pipeline解决**

*   Pipeline 主要就是为了解决存在这种情况的场景，使用Pipeline模式，客户端可以一次性发送多个命令，无需等待服务端返回，这样可以将多次I/O往返的时间缩短为一次，大大减少了网络往返时间，提高了系统性能。
*   Pipeline 是基于队列实现，基于先进先出的原理，满足了数据顺序性。同时一次提交的命令很多的话，队列需要非常大量的内存来组织返回数据内容，如果大量使用Pipeline的话，应当合理分批次提交命令。
*   Pipeline的默认同步个数为`53`个，累加到 53 条数据时会把数据提交

*注意：* Redis 集群中使用不了 Pipeline，对可靠性要求很高，每次操作都需要立即获取本次操作结果的场景都不适合用 Pipeline

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-a5a7ee47-f220-44bf-ab9d-fb90683c4b46.jpg)

1.  Master 最好不要做 RDB 持久化，因为这时 save 命令调度 rdbSave 函数，会阻塞主线程的工作，当数据集比较大时可能造成主线程间断性暂停服务
2.  如果数据比较重要，将某个 Slave 节点开启AOF数据备份，策略设置为每秒一次
3.  为了主从复制速度和连接的稳定性，Master 和 Slave 最好在同一个局域网中
4.  尽量避免在运行压力很大的主库上增加从库
5.  主从复制不要用图状结构，用单向链表结构更为稳定，`Mater->Slave1->Slave2->Slave3...` 这样的结构方便解决单点故障问题，实现 Slave 对 Master 的替换，如果 Master 崩溃，可以立即启用 Slave1替换Mater，而其他依赖关系则保持不变。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-433affa2-0cf5-42d2-9101-4d4f297055a9.jpg)

**方式1：先更新数据库，再更新缓存**

这种是常规的做法，但是如果更新缓存失败，将会导致缓存是旧数据，数据库是新数据

**方式2：先删除缓存，再写入数据库**

这种方式能够解决方式1的问题，但是仅限于低并发的场景，不然如果有新的请求在删完缓存之后，写数据库之前进来，那么缓存就会马上更新数据库更新之前数据，造成数据不一致的问题

**方式3：延时双删策略**

这种方式是先删除缓存，然后更新数据库，最后延迟个几百毫秒再删除缓存

**方式4：直接操作缓存，定期写入数据库**

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-978967aa-4b5a-4a0b-b2bd-78d726e01125.jpg)

> 虽然Redis的Transactions 提供的并不是严格的 ACID的事务（如一串用EXEC提交执行的命令，如果在执行中服务器宕机，那么会有一部分命令执行一部分命令未执行），但这些Transactions还是提供了基本的命令打包执行的功能（在服务器不出问题的情况下，可以保证一连串的命令是顺序在一起执行的。

Redis 事务的本质就是四个原语：

1.  `multi`：用于开启一个事务，它总是返回 OK，当 multi 执行之后，客户端可以继续向服务器发送任意多条命令，这些命令不会被立即执行，而是放到一个队列中，当 exec 命令被调用的时候，所有队列d 命令才会执行
2.  `exec`：执行所有事务队列内的命令，返回事务内所有命令的返回值，当操作被打断的时候，返回空值 nil
3.  `watch`：是一个乐观锁。可以为 redis 事务提供 CAS 操作，可以监控一个或多个键。一旦其中有一个键被修改（删除），之后的事务就不会执行，监控一直持续到 exec 命令执行之后
4.  `discard`：调用 discard，客户端可以清空事务队列中的命令，并放弃执行事务

事务支持一次执行多个命令，一个事务中的所有命令都会被序列化。在事务执行的过程中，会按照顺序串行化执行队列中的命令，其他客户端提交的命令请求不会插入到事务执行命令队列中。Redis 不支持回滚事务，在事务失败的时候不会回滚，而是继续执行余下的命令。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-272db8a6-0e11-4a45-8f59-904a17b09ad4.jpg)

**方式1：Cluster 3.0**

这是Redis 自带的集群功能，它采用的分布式算法是哈希槽，而不是一致性Hash。支持主从结构，可以扩展多个从服务器，当主节点挂了可以很快切换到一个从节点作主节点，然后其他从节点都会切换读取最新的主节点。

**方式2：Twemproxy**

Twitter 开源的一个轻量级后端代理。可以管理 Redis 或 Memcache 集群。相对于 Redis 集群来说，易于管理。它的使用方法和Redis集群没有任何区别，只需要设置多个Redis实例后，在本需要连接 Redis 的地方改为连接 Twemproxy ，它就会以一个代理的身份接收请求并使用一致性Hash算法，将请求连接到具体的Redis节点上，将结果再返回Twemproxy。对于客户端来说，Twemproxy 相当于是缓存数据库的总入口，它不需要知道后端如何部署的。Twemproxy 会检测与每个节点的连接是否正常，如果存在异常节点就会将其剔除，等一段时间后，Twemproxy 还会再次尝试连接被剔除的节点。

**方式3：Codis**

它是一个 Redis 分布式的解决方法，对于应用使用 Codis Proxy 的连接和使用Redis的服务没有明显区别，应用能够像使用单机 Redis 一样，让 Codis 底层处理请求转发，实现不停机实现数据迁移等工作。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-cb4dd9db-cc87-413c-8f4f-257b215ff7ef.jpg)

> 什么是脑裂问题

脑裂问题通常是因为网络问题导致的。让 master、slave 和 sentinel 三类节点处于不同的网络分区。此时哨兵无法感知到 master 的存在，会将 slave 提升为 master 节点。此时就会存在两个 master，就像大脑分裂，那么原来的客户端往继续往旧的 master 写入数据，而新的master 就会丢失这些数据

> 如何解决

通过配置文件修改两个参数

```
min-slaves-to-write 3  # 表示连接到 master 最少 slave 的数量
min-slaves-max-lag 10  # 表示slave连接到master最大的延迟时间
--------------------新版本写法-----------------
min-replicas-to-write 3
min-replicas-max-lag  10
```

配置这两个参数之后，如果发生集群脑裂，原先的master节点接收到写入请求就会拒绝，就会减少数据同步之后的数据丢失

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-bafd12cc-fc0c-4951-bfb5-ec5e8f609032.jpg)

一般使用 `List` 结构作为队列。`Rpush` 生产消息，`Lpop` 消费消息。当 `Lpop` 没有消费的时候，需要适当 sleep 一会再重试。但是重复 sleep 会耗费性能，所以我们可以利用 list 的 `blpop` 指令，在还没有消息到来时，它会阻塞直到消息到来。

我们也可以使用 `pub/sub` 主题订阅者模式，实现 1：N 的消费队列，但是在消费者下线的时候，生产的消息会丢失

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-55366226-cdcb-49e7-ba44-93adba240c4f.jpg)

可以使用 `zset` 结构，可以拿时间戳作为 score，消息的内容作为key，通过调用 `zadd` 来生产消息，消费者使用 `zrangebyscore` 指令轮询获取 N 秒之前的数据进行处理

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-e57e2e25-5c59-4f98-9f51-e2cce8bf1317.jpg)

Redis Cluster提供了自动将数据分散到各个不同节点的能力，但采用的策略并不是一致性Hash，而是哈希槽。Redis 集群将整个Key的数值域分成16384个哈希槽，每个Key通过 CRC16检验后对16384驱魔来决定放置到那个槽中，集群的每个节点都负责其中一部分的哈希槽。

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-4a0ddadf-45f6-44e8-a696-c159c0ca2759.jpg)

**1、数据缓存**

经典的场景，现在几乎是所有中大型网站都在用的提升手段，合理地利用缓存能够提升网站访问速度

**2、排行榜**

可以借助Redis提供的有序集合（`sorted set`）能力实现排行榜的功能

**3、计数器**

可以借助Redis提供的 `incr` 命令来实现计数器功能，因为是单线程的原子操作，保证了统计不会出错，而且速度快

**4、分布式session共享**

集群模式下，可以基于 Redis 实现 session 共享

**5、分布式锁**

在分布式架构中，为了保证并发访问时操作的原子性，可以利用Redis来实现分布式锁的功能

**6、最新列表**

可以借助Redis列表结构，`LPUSH`、`LPOP`、`LTRIM`等命令来完成内容的查询

**7、位操作**

可以借助Redis中 `setbit`、`getbit`、`bitcount` 等命令来完成数量上千万甚至上亿的场景下，实现用户活跃度统计

**8、消息队列**

Redis 提供了发布（`Publish`）与订阅（`Subscribe`）以及阻塞队列能力，能够实现一个简单的消息队列系统

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-320f06ce-7aee-4011-bf2a-067c23cc222b.jpg)

**方式1：Set 结构**

以日期为 key，以用户 ID（对应数据库的 Primary Id）组成的集合为 value

*   查询某个用户的签到状态 `sismember key member`
*   插入签到状态 `sadd key member`
*   统计某天用户的签到人数 `scard key`

##### 2、bitMap 结构

Key的格式为`u:sign:uid:yyyyMM`，Value则采用长度为4个字节（32位）的位图（最大月份只有31天）。位图的每一位代表一天的签到，1表示已签，0表示未签。

```
# 用户2月17号签到
SETBIT u:sign:1000:201902 16 1 # 偏移量是从0开始，所以要把17减1


# 检查2月17号是否签到
GETBIT u:sign:1000:201902 16 # 偏移量是从0开始，所以要把17减1


# 统计2月份的签到次数
BITCOUNT u:sign:1000:201902


# 获取2月份前28天的签到数据
BITFIELD u:sign:1000:201902 get u28 0


# 获取2月份首次签到的日期
BITPOS u:sign:1000:201902 1 # 返回的首次签到的偏移量，加上1即为当月的某一天
```

***两者对比***

1.  使用 set 的方式所占用的内存只与数量相关，和存储哪些 ID 无关
2.  使用 bitmap 的方式所占用的内存与数量没有绝对的关系，而是与最高位有关，比如假设 ID 为 500 W的用户签到了，那么从 1~4999999 用户不管是否签到，所占用的内存都是 500 w个bit，这边是最坏的情况
3.  使用 bitmap 最大可以存储 2^32-1也就是 512M 数据
4.  使用 bitmap 只适用存储只有两个状态的数据，比如用户签到，资源（视频、文章、商品）的已读或未读状态

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-dbdb69a5-1043-4a41-84c4-196f19746cef.jpg)

Redis中 ZSet 是选择使用 `跳表` 而不是红黑树

> 什么是跳表

*   跳表是一个随机化的数据结构，实质上就是一种可以进行二分查找的有序链表。
*   跳表在原有的有序链表上增加了多级索引，通过索引来实现快速查找
*   跳表不仅能提高搜索性能，同时也可以提高插入和删除操作的性能

![](https://cdn.tobebetterjavaer.com/tobebetterjavaer/images/nice-article/weixin-baogwdredismsgczhs-a98a9de1-c42a-481e-908b-57632ce9fdcc.jpg)

***总结：***

1.  跳表是可以实现二分查找的有序链表
2.  每个元素插入时随机生成它的 level
3.  最底层包含所有的元素
4.  如果一个元素出现在 level(x)，那么它肯定出现在 x 以下的 level 中
5.  每个索引节点包含两个指针，一个向下，一个向右
6.  跳表查询、插入、删除的时间复杂度为 O(log n)，与平衡二叉树接近

> 为什么不选择红黑树来实现

首先来分析下 Redis 的有序集合支持的操作：

1.  插入元素
2.  删除元素
3.  查找元素
4.  有序输出所有元素
5.  查找区间内的所有元素

其中前 4 项红黑树都可以完成，且时间复杂度与跳表一致，但是最后一个红黑树的效率就没有跳表高了。在跳表中，要查找区间的元素，只要定位到两个区间端点在最低层级的位置，然后按顺序遍历元素就可以了，非常高效。


---

没有什么使我停留——除了目的，纵然岸旁有玫瑰、有绿荫、有宁静的港湾，我是不系之舟🤔。

- ✌️：[工作四年，被动醒悟](https://mp.weixin.qq.com/s/q-o4SBZQ3SH62T0c52aBUw)
- ✌️：[来网易四个月，真的不一样](https://mp.weixin.qq.com/s/4Zcd16hMazydelrN6CUXBA)
- ✌️：[秋招 13 家 offer，手到擒来](https://mp.weixin.qq.com/s/LKkvcSdhMyXAGgtqEak0Zw)
- ✌️：[考研失败，真的不甘心](https://mp.weixin.qq.com/s/mrSxrQYaWiUE82tBUu3XIw)
- ✌️：[想春招找个实习，我该如何准备？](https://mp.weixin.qq.com/s/eyCEQKclRkTnsJze01Bilg)
- ✌️：[逼签！冲字节还是苟同花顺？](https://mp.weixin.qq.com/s/Dv_kcwUT-KTZ6LAwn3o_Jg)
- ✌️：[简历上写了这俩项目，超级加分！](https://mp.weixin.qq.com/s/yYWD0VPZ_NoGPOmhoa-OlQ)
- ✌️：[双非很菜，拿到这俩offer挺不容易](https://mp.weixin.qq.com/s/OHXpEOKcLaKW8h0TS4Xqjg)
- ✌️：[今年嵌入式软件这块真挺香](https://mp.weixin.qq.com/s/6YuyA1Ja5RfDEQatfsQpjA)
- ✌️：[入职 15 天，就想跑路了？](https://mp.weixin.qq.com/s/EW95wdK4SM0CiBEJqUP7Mg)
- ✌️：[比亚迪，救了我秋招的命](https://mp.weixin.qq.com/s/PmVwFKsXkGeJjmNPiu5hrQ)


>参考链接：[https://mp.weixin.qq.com/s/l9V21i70cMlq3mE_4cA2oA](https://mp.weixin.qq.com/s/l9V21i70cMlq3mE_4cA2oA)，出处：菜农曰，整理：沉默王二
