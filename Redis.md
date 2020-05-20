# Redis

## Redis安装

​						首先需要保证运行机上有gcc环境，进行编译安装，也可以通过docker进行安装。

```xml
$cd redis
$make
$make PERFIX=/uer/local/redis install
```

启动Redis客户端命令：

```
redis-cli -h IP -p 端口  //默认IP为本机  端口为6379
```

## Reids数据类型

Redis提供string、hash、list、set、sorted set五中数据类型。

![image-20200507214700237](F:\Document\note\jimages\redis-data-type.png)

### string 类型

string类型是最基本的类型，而且string类型是二进制安志全的。redis的string可以包含任何数据，可以是jpg图片或者序列化的对象。从内部来看，string可以看做byte数组，最大上限是1G字节。 一个键最大能存储512M。

string类型数据操作指令简介：

- set key value

  设置key对应的string类型的值，返回1表示成功，返回0表示失败。

- setnx key value 

  如果key不存在，设置key对应string类型的值；如果key值已经存在，返回0。

- get key 

  获取key对应的string值，如果key值不存在返回nil。

- getset key value

  先获取key的值，再设置key的值，如果key不存在返回nil。

- mget key1 key2 ......keyN 

  一次获取多个key的值，如果对应的key不存在，则对应返回nil。

- mset key1 key2 ......keyN 

  一次设置多个key的值，成功返回1表示所有的值都设置了，失败返回0表示没有任何值被设置。

- msetnx key1 value1 ...... keyN valueN

  一次设置多个key的值，但是不会覆盖已经存在的key。

- incr key

  对key的值做++操作，并返回新的值。【注意】：incr一个不是int的value会返回报错，incr一个不存在的key,则设置key的值为1。

- decr key

  对key的值做--操作，decr一个不存在的key,则设置key的值为-1。

- incrby key integer

  对key加上指定值，key不存在时会设置key。并认为原来的value是0。

- decrby key integer

  对key减去指定值。decrby完全是为了可读性，我们完全可以通过incrby一个负值来实现同样效果，反之一样。

### hash类型

hash 是一个 string 类型的 field 和 value 的映射表。添加，删除操作都是 O(1)（平均）。 hash 特别适合用于存储对象。相对于将对象的每个字段存成单个 string 类型。将一个对象存储在 hash 类型中会占用更少的内存，并且可以更方便的存取整个对象。省内存的原因是新建一个 hash 对象时开始是用 zipmap（又称为 smallhash）来存储的。这个 zipmap 其实并不是hash table，但是 zipmap 相比正常的 hash 实现可以节省不少 hash 本身需要的一些元数据存储开销。尽管zipmap 的添加，删除，查找都是O(n)，但是由于一般对象的 field 数量都不太多。所以使用 zipmap 也是很快的,也就是说添加删除平均还是 O(1)。如果 field 或者value的大小超出一定限制后，redis会在内部自动将zipmap替换成正常的hash实现. 这个限制可以在配置文件中指定。

hash-max-zipmap-entries 64  #配置字段最多为64个

hash-max-zipmap-value 512	#配置value最大为512字节

hash类型数据操作指令简介

- hset key field value 

  设置hash field为指定值，如果key不存在，则创建。

- hget key field

  获取指定的hash field。

- hmget key filed1 .....filedN

  获取全部指定的hash field。

- hmset key filed1 value1 ..... fieldN valueN

  同时设置hash的多个field。

- hincrby key field integer

  将指定的hash field加上指定值。成功后返回hash field变更后的值。

- hexists key field

  检测指定的field是否存在。

- hdel key field

  删除指定的hash field。

- hlen key

  返回指定hash的field数量。

- hkeys key

  返回hash的所有field。

- hvals key

  返回hash的所有value。

- hgettall

  返回hash的所有field和value。

### list类型

list是一个链表结构，可以理解为一个每个子元素都是string类型的双向链表。主要功能是push、pop、获取一个范围的所有值等。操作中key理解为链表的名字。

list类型数据操作指令简介

- lpush  key string

  在key对应list的头部添加字符串元素，返回1表示成功，0表示key存在且不是list类型。

- rpush key string

  在key对应list的尾部添加字符串元素。

- llen key

  返回key对应list的长度，如果key不存在返回0，如果key对应类型不是list返回错误。

- lrange key start end

  返回指定区间内的元素，下标从0开始，负值表示从尾计算，-1表示倒数第一个元素，key不存在返回空列表。

- ltrim key start end 

  截取list指定区间内元素，成功返回1，key值不能存在返回错误。

- lset key index value 

  设置list中指定下表的元素值，成功返回1，key或者下标不存在返回错误。

- lrem key count value 

  从list的头部或尾部删除一定数量匹配value的元素，返回删除的元素数量。count为0的时候删除全部。

- lpop key

  从list的头部删除并返回删除元素。如果key对应list不存在或者是空返回nil,如果key对应的值不是list返回错误。

- rpop  key

  从list的尾部删除并返回删除元素。

- blpop  key1 .....keyN timeout 从左到右扫描，返回对第一个非空 list 进行 lpop 操作并返回，比如 blpop list1 list2 list3 0 ,如果 list 不存在 list2,list3 都是非空则对 list2做 lpop 并返回从 list2 中删除的元素。如果所有的 list 都是空或不存在，则会阻塞 timeout 秒，timeout 为0表示一直阻塞。当阻塞时，如果有client 对 key1...keyN中的任意 key进行push 操作，则第一在这个key上被阻塞的client会立即返回。如果超时发生，则返回 nil。有点像 unix 的 select 或者 poll。

- brpop 同blpop,一个从头部删除一个是从尾部删除。

### set类型

set 是无序集合，最大可以包含(2 的 32 次方-1)个元素。set 的是通过 hashtable 实现的， 所以添加，删除，查找的复杂度都是 O(1)。hashtable 会随着添加或者删除自动的调整大小。 需要注意的是调整 hashtable 大小时候需要同步（获取写锁）会阻塞其他读写操作。可能不 久后就会改用跳表（skip list）来实现。跳表已经在 sortedsets 中使用了。关于 set 集合类型 除了基本的添加删除操作，其它有用的操作还包含集合的取并集(union)，交集(intersection)， 差集(difference)。

set 类型数据操作指令简介 

- sadd key member 

  添加一个 string 元素到 key 对应 set 集合中，成功返回 1,如果元素以及 在集合中则返回 0，key 对应的 set 不存在则返回错误。

- srem key member 

  从 key 对应 set 中移除指定元素，成功返回 1，如果 member 在集合中不 存在或者 key 不存在返回 0，如果 key 对应的不是 set 类型的值返回错误。

- spop key

  删除并返回 key 对应 set 中随机的一个元素,如果 set 是空或者 key 不存在返回 nil。

- srandmember key

  同 spop，随机取 set 中的一个元素，但是不删除元素。

-  smove srckey dstkey member

  从srckey对应set中移除member并添加到dstkey对应set中， 整个操作是原子的。成功返回 1,如果 member 在 srckey 中不存在返回 0，如果 key 不是 set 类型返回错误。

- scard key 

  返回 set 的元素个数，如果 set 是空或者 key 不存在返回 0。

- sismember key member 

  判断 member 是否在 set 中，存在返回 1，0 表示不存在或者 key 不 存在。

- sinterkey1 key2 ……keyN 

  返回所有给定 key 的交集。

-  sinterstore dstkey key1 .......keyN

  返回所有给定key的交集，并保存交集存到dstkey 下。

- sunion key1 key2 ......keyN

  返回所有给定 key 的并集。

- sunion store dstkey key1 ......keyN 

  返回所有给定 key 的并集，并保存并集到 dstkey 下。

- sdiff key1 key2......keyN 

  返回所有给定 key 的差集。

- sdiff store dstkey key1 ......keyN

   返回所有给定 key 的差集，并保存差集到 dstkey 下。

- smembers key 

  返回 key 对应 set 的所有元素，结果是无序的。

### sorted set类型

sorted set 是有序集合，它在 set 的基础上增加了一个顺序属性，这一属性在添加修 改元素的时候可以指定，每次指定后，会自动重新按新的值调整顺序。可以理解了有两列的 mysql 表，一列存 value，一列存顺序。操作中 key 理解为 sorted set 的名字。

Sorted Set 类型数据操作指令简介

- add key score member 

  添加元素到集合，元素在集合中存在则更新对应 score。

-  zrem key member

   删除指定元素，1 表示成功，如果元素不存在返回 0。 

-  zincrby  key incrmember 

  增加对应 member 的 score 值，然后移动元素并保持 skiplist 保持有 序。返回更新后的 score 值。 

- zrank key member 

  返回指定元素在集合中的排名（下标），集合中元素是按 score 从小到大 排序的。

- zrevrank key member

   同上,但是集合中元素是按 score 从大到小排序。

-  zrange key startend 

  类似 lrange 操作从集合中去指定区间的元素。返回的是有序结果。

- zrevrange key startend 

  同上，返回结果是按 score 逆序的。

-  zrangebyscore key min max 

  返回集合中 score 在给定区间的元素。 

- zcount key min max 

  返回集合中 score 在给定区间的数量。 

- zcard key 

  返回集合中元素个数。

-  zscore key element 

  返回给定元素对应的 score。

- zremrangebyrank key min max

   删除集合中排名在给定区间的元素。 

- zremrangebyscore key min max 

  删除集合中 score 在给定区间的元素。

### 数据类型应用场景

![image-20200507214911467](F:\Document\note\jimages\use_env.png)

## 持久化

​			通常Redis将数据存储在内存中或虚拟内存中，它是通过以下两种方式实现对数据的持久化。

### 快照方式(默认持久化方式)

快照方式就是将内存中数据以快照的方式写入到二进制文件中，默认的文件名为 dump.rdb。 客户端也可以使用 save 或者 bgsave 命令通知 redis 做一次快照持久化。save 操作是在主线程中保存快照的，由于 redis 是用一个主线程来处理所有客户端的请求，这种方式会阻塞所有客户端请求。所以不推荐使用。另一点需要注意的是，每次快照持久化都是将内存数 据完整写入到磁盘一次，并不是增量的只同步增量数据。如果数据量大的话，写操作会比较 多，必然会引起大量的磁盘 IO 操作，可能会严重影响性能。 

![image-20200507215415044](F:\Document\note\jimages\redis-rdb.png)

【注意】：由于快照方式是在一定间隔时间做一次的，所以如果 redis 意外当机的话，就会 丢失最后一次快照后的所有数据修改。

### 日志追加方式

日志追加方式 redis 会将每一个收到的写命令都通过 write 函数追加到文件中(默认 appendonly.aof)。当 redis 重启时会通过重新执行文件中保存的写命令来在内存中重建整个数据库的内容。当然由于操作系统会在内核中缓存 write 做的修改，所以可能不是立即写 到磁盘上。这样的持久化还是有可能会丢失部分修改。不过我们可以通过配置文件告诉 redis 我们想要通过 fsync 函数强制操作系统写入到磁盘的时机。有三种方式如下（默认是： 每秒 fsync 一次）
appendonly yes    //启用日志追加持久化方式 

#appendfsync always    //每次收到写命令就立即强制写入磁盘，最慢的，但是保证完全 的持久化，不推荐使用 。

#appendfsync everysec   //每秒钟强制写入磁盘一次，在性能和持久化方面做了很好的折 中，推荐 

#appendfsync no   //完全依赖操作系统，性能最好,持久化没保证日志追加方式同时带来了另一个问题。持久化文件会变的越来越大。例如我们调用 incr test 命令 100 次，文件中必须保存全部 100 条命令，其实有 99 条都是多余的。因为要恢复数据库状态其实文件中保存一条settest100就够了。为了压缩这种持久化方式的日志文件。 redis 提供了bgrewriteaof命令。收到此命令 redis 将使用与快照类似的方式将内存中的数据 以命令的方式保存到临时文件中，最后替换原来的持久化日志文件。

![image-20200507215507325](F:\Document\note\jimages\redis-aof.png)

两种持久化的区别

![image-20200507215647366](F:\Document\note\jimages\redis-diff-rdb-aof.png)

【注】：RESP是redis客户端和服务端之间使用的一种通讯协议；RESP实现简单，快速解析、可读性好。

## 内存淘汰策略：

1. volatile-lru:从已设定超时时间的数据，淘汰最不常使用的数据。
2. allkeys-lru:从所有数据集中挑选最近最少少使用的数据进行淘汰，这是最广泛的策略。
3. volatile-random:从设定了超时的数据中随机进行淘汰。
4. allkeys-random:查询所有数据集中随机进行淘汰。
5. volatile-ttl:查询全部设定超时时间的数据之后排序，将马上要过期的数据进行淘汰。
6. noeviction:如果设置为该属性，则不会进行淘汰操作，如果内存溢出则返回报错。
7. volatile-lfu:从所有配置了过期时间的键中驱逐使用频率最少的键。
8. allkeys-lfu:从所有键中驱逐使用频率最少的键。

## 事务

事务中的多个命令被一次性发送给服务器，而不是一条一条的发送，这种方式称为流水线，它可以减少客户端与服务器之间的网络通信次数而提升性能。

**Redis最简单的事务实现方式是使用MULTI和EXEC命令将事务操作包围起来。**

它先以MULTI开始一个事务。然后将多个命令入队到事务中，最后由EXEC命令触发事务，一并执行事务中的所有命令。单个Redis命令执行是原子性的，但是Redis没有在事务上增加任何维持原子性的机制，所以Redis的事务执行并不是原子性的。Redis事务可以理解为一个打包的批量执行脚本，但批量指令并非原子化的操作，中间某条指令的失败不会导致前面已做的指令回滚，也不会造成后面的指令不做。

## 事件

Redis服务器是一个事件驱动程序。

### 文件事件

服务器通过套接字与客户端或者其他服务器进行通信，文件事件就是对套接字操作的抽象。

Redis基于Reactor模式开发了自己的网络时间处理器，使用I/O多路复用程序来同时监听多个套接字，并将到达的时间传送给文件分派器，分派器会根据套接字产生的事件类型调用相应的事件处理器。

### 时间事件

服务器有一些操作需要在给定的时间点执行，时间事件是这类定时操作的抽象。

时间事件又分为：

- 定时事件：是让一段程序在指定的时间之内执行一次。

- 周期性事件：让一段程序每隔指定时间就执行一次。

Redis将所有时间事件都放在一个无序链表中，通过遍历整个链表查找出已到达时间事件，并调用相应的事件处理器。

## Redis的Java简单运用

```java
import java.util.Iterator;
import java.util.List;
import java.util.Set;

import redis.clients.jedis.Jedis;

/**
 * @Description:
 * @Author: Vangen
 * @Date: 2020/5/7 21:21
 * @Version: 1.0
 */
public class RedisJava {
    public static void main(String[] args) {
        /**
         * 连接本地redis服务
         */
        Jedis jeddis = new Jedis("localhost");
        System.out.println("连接成功");
        /**
         * 检查服务是否运行
         */
        System.out.println("服务正在运行:" + jeddis.ping());
        /**
         * 设置 redis 字符串数据
         */
        jeddis.set("vangen", "王根");
        /**
         * 获取存储的数据并输出
         */
        System.out.println(jeddis.get("vangen"));
        /**
         * 存储数据到list列表中
         */
        jeddis.lpush("site-list", "王根");
        jeddis.lpush("site-list", "Google");
        jeddis.lpush("site-list", "Baidu");
        // 获取存储的数据并输出
        List<String> list = jeddis.lrange("site-list", 0, 2);
        for (int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }

        /**
         * 获取所有的keys，并输出
         */
        Set<String> keys = jeddis.keys("*");
        Iterator<String> iterator = keys.iterator();
        while (iterator.hasNext()) {
            String key = iterator.next();
            System.out.println(key);
        }

        /**
         * 删除key
         */
       long k=  jeddis.del("i");
        System.out.println(k);
    }
}
```

