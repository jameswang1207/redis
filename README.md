# No sql
   - 泛指非关系型数据库，存储的数据不要f固定的模式，无需多余的操作就可以横向扩展。
# 去哪里：
   - 专做高速缓存：memcache
   - 多种用途： redis
# redis干什么：K -V Cache persistence
    - redis的三个特点
        - Redis 支持数据的持久性，可将内存中的数据保存在磁盘中，重启可再次加载使用
        - Redis不仅支持简单的 k-v 键值对，还支持list set zset hash 等数据结构的存储
        - Redis支持出具的备份，即master-slave模式的数据备份

# 互联网中的3v + 3高
    - 3v : 海量（朋友圈） 多样（微博中的内容：文字，图片，音频等） 实时
    - 3高： 高并发 高可扩 高性能

# 当下的Nosql的经典应用：
    - 一个电商客户，订单，订单商品，地址模型来对比关系型数据库和非 关系型数据库
        - 关系型数据库：分和不易
        - 非关系型数据库：分和简单
    - 高并发操作不建议有关联查询
    - 互联网公司进行数据冗余来避免关联查询

# NoSQL 简介：
    - NoSQL 数据模型：kv键值 | bson | 列族 | 图形
    - NoSQL 数据库的四大分类：
       - kv键值：redis 
       - 文档数据库： mongodb
       - 列存储数据库：Hbase，分布是文件系统
       - 图关系数据库：

    - Redis__分布式数据库CAP原理
        - 传统数据库ACID
            - Atomicity:原子性
            - Consistency:一致性
            - Isolation:独立性
            - Durability: 持久性
        - CAP:
            - Consistancy: (强制一致性)
            - Availablity: (可用性)
            - Partition tolerance(分区容错性)
        - CAP 只能三进二
            - CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，
        - BASE
            - 就是为了解决关系数据库强一致性引起的问题而引起的可用性降低而提出的解决方案。
            - BASE其实是下面三个术语的缩写：
                - 基本可用（Basically Available）
                - 软状态（Soft state）
                - 最终一致（Eventually consistent）

    - 分布式 && 集群
        - 分布式： 不同的多台服务器上面部署不同的服务模块（工程），他们之间通过RpcRmi之间通信和调用，对外提供服务和组内协作。
        - 集群： 不同的多台服务器上面部署相同的服务模块，通过分布式调度软件进行统一的调度，对外提供服务和访问。

    - redis安装：
        - 下载redis tar 包
        - cd redis-3.2.4
        - make | 有可能会出现gcc失败（安装gcc）
            ```shell
                cd src && make all
                make[1]: Entering directory `/opt/redis-3.2.4/src'
                Hint: It's a good idea to run 'make test' ;)
                make[1]: Leaving directory `/opt/redis-3.2.4/src
            ```
            - 运行make distclean 后再make
        - make install
        - cd redis-3.2.4将redis.conf中的 daemonize no 设置为 daemonize yes：将redis作为后台线程运行
        - 测试redis性能：/usr/local/bin 运行redis-benchmark

    - redis 使用命令
        - select : 切换数据库
        - dbsize：查看当前数据库中的key的数量
        - flushdb : 清空当前的库
        - flushall: 清空全部的库
        - redis : 默认端口6379

    - redis 数据类型
        -  string：
            - string是redis最基本的类型，你可以理解成与Memcached一模一样的类型，一个key对应一个value。
            - string类型是二进制安全的。意思是redis的string可以包含任何数据。比如jpg图片或者序列化的对象 。
            - string类型是Redis最基本的数据类型，一个redis中字符串value最多可以是512M
        - Hash：
            - 是一个string类型的field和value的映射表，hash特别适合用于存储对象
            - 类似Java里面的Map<String,Object>
        - List（列表）
            - Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素导列表的头部（左边）或者尾部（右边）。它的底层实际是个链表
        - Set（集合）
            - Redis的Set是string类型的无序集合。它是通过HashTable实现实现的，
            - zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。
            - 不同的是每个元素都会关联一个double类型的分数。
            - redis正是通过分数来为集合中的成员进行从小到大的排序。
            - zset的成员是唯一的,但分数(score)却可以重复

    - （redis命令参考）http://redisdoc.com/
        - redis key: 
            - 判断某个key是否存在：exists key的名字
            - 将某个移动到另外的库中：move key 2 ： 从当前库移动到2号库中
            - 给某个key设置过期时间： exprie  key 秒钟：为当前的key设置过期时间
            - 查看某个key还有多少秒过期。
            - type key查看你的key是什么类型
        - list： 
            - lpush|rpush|lrange（查询） : rpush : 怎样进，怎样出, 其他的先进后出.
            - lpop (向左出栈) ， rpop(向右出栈).
            - lindex(下标索引)： lindex  list1  1 .
            - llen (查看某个栈的长度) llen list1. 
            - lrem key 删除N个key (lrem list1 3 8)(删除list1中的三个8)
            - ltrim key  截取index到index的值 并将这个值赋给当前的key（ltrim list1 2 8）
            - RPOPLPUSH：（RPOPLPUSH list1 list2）(list1 拿出一个进入list2中)
            - lset key index 【value】 (修改某个元素的值)：lset list1 1 x (将第1个元素设置为x)
        - list 性能总结：
            - 它是一个字符串链表，left、right都可以插入添加；
            - 如果键不存在，创建新的链表；如果键已存在，新增内容；
            - 如果值全移除，对应的键也就消失了。
            - 链表的操作无论是头和尾效率都极高，但假如是对中间元素进行操作，效率就很惨淡了。

        - Set
            - sadd|smembers|sismember(sadd set0 value)(自动去重)
            - scard (获取集合中的元素个数)
            - srem key value 删除集合中的元素
            - srandmember key 某个整数（随机出几个数）（srandmember set0 2）(从set0中随机出两个数)
            - spop key 随机出栈 (spop set0)
            - smove key1 key2 在key1中的某个值（将key1中的某个值赋给key2）
                SMOVE set0  set1 james
            - 数学集合
               差集：sdiff (sdiff set0 set1 在第一个set里,但不在后面任何一个set里)
               交集：sinter（sinter set0 set1）
               并集：sunion
        - hash-------
            - KV模式不变，但key是一个键值对
                - hset|hget|hmset|hmget|hgetall|hdel
                    - hset user id 11 ->  hget user id
                    - hmset user id 11 name james age 12 -> hmget user id name age -> hgetall user
                - hlen : 获得redis的长度
                - HEXISTS (hexists user name 判断某个值是否存在)
                - hkeys : hkeys user 
                - hvals : hvals user
                - hincrby|hincrbyfloat:(HINCRBY user age 3)
                - hsetnx : HSETNX user age 12 (如果存在插入失败，不存在插入成功)
            zset(有序集合)---做统计
                - zadd|zrange 
                    - （zadd zset0 20 v1 30 v2）
                    - （ZRANGE zset0 0 -1 ）{只去除值} ZRANGE zset0 0 -1 withscores（去除全部）
                - zrangebyscore key 开始score 结束score
                    - ZRANGEBYSCORE zset0 20 30
                - zrem key 某个score下对应的value值，作用删除元素
                    - zrem zset0 v1
                - zcard |zcount key score 区间|zrank key values值 作用是获得下表对应的值 | zscore key 对应的值
                    - zcard zset0
                    - ZCOUNT zset0 20 30
                    - ZRANK zset0 v4
                    - ZSCORE zset0 v4
                - zrevrange : (ZREVRANGE zset0  20  30)
                - zrevrangebyscore key : (ZREVRANGEBYSCORE zset0 30  20)

    - （redis的配置redis.conf）
        - config get requirepass (获得用户名和密码)
        - config set requirepadd  "james" (设置用户名和密码)
        - Maxmemory-samples 
            - lru：最近最久未使用
            - volatile-lru -> 使用LRU算法移除key，只对设置了过期时间的键
            - allkeys-lru -> 使用LRU算法移除key
            - volatile-random -> 在过期集合中移除随机的key，只对设置了过期时间的键
            - allkeys-random -> 移除随机的key
            - volatile-ttl -> 移除那些TTL值最小的key，即那些最近要过期的key
            - noeviction -> 不进行移除。针对写操作，只是返回错误信息
        - maxmemory-sample
            - 设置样本数量，LRU算法和最小TTL算法都并非是精确的算法，而是估算值，所以你可以设置样本的大小，redis默认会检查这么多个key并选择其中LRU的那个
        -redis.conf 配置项说明如下：
            - Redis默认不是以守护进程的方式运行，可以通过该配置项修改，使用yes启用守护进程
                - daemonize no
            - 当Redis以守护进程方式运行时，Redis默认会把pid写入/var/run/redis.pid文件，可以通过pidfile指定
                - pidfile /var/run/redis.pid
            - 指定Redis监听端口，默认端口为6379，作者在自己的一篇博文中解释了为什么选用6379作为默认端口，因为6379在手机按键上MERZ对应的号码，而MERZ取自意大利歌女Alessia Merz的名字  
                - port 6379
            - 绑定的主机地址  bind 127.0.0.15.当客户端闲置多长时间后关闭连接，如果指定为0，表示关闭该功能  timeout 300
            - 指定日志记录级别，Redis总共支持四个级别：debug、verbose、notice、warning，默认为verbose  loglevel verbose
            - 日志记录方式，默认为标准输出，如果配置Redis为守护进程方式运行，而这里又配置为日志记录方式为标准输出，则日志将会发送给/dev/null  logfile stdout
            - 设置数据库的数量，默认数据库为0，可以使用SELECT <dbid>命令在连接上指定数据库id  databases 16
            - 指定在多长时间内，有多少次更新操作，就将数据同步到数据文件，可以多个条件配合  save <seconds> <changes>  Redis默认配置文件中提供了三个条件：  save 900 1  save 300 10  save 60 10000  分别表示900秒（15分钟）内有1个更改，300秒（5分钟）内有10个更改以及60秒内有10000个更改。
            - 指定存储至本地数据库时是否压缩数据，默认为yes，Redis采用LZF压缩，如果为了节省CPU时间，可以关闭该选项，但会导致数据库文件变的巨大  rdbcompression yes
            - 指定本地数据库文件名，默认值为dump.rdb  dbfilename dump.rdb
            - 指定本地数据库存放目录  dir ./
            - 设置当本机为slav服务时，设置master服务的IP地址及端口，在Redis启动时，它会自动从master进行数据同步  slaveof <masterip> <masterport>
            - 当master服务设置了密码保护时，slav服务连接master的密码  masterauth <master-password>
            - 设置Redis连接密码，如果配置了连接密码，客户端在连接Redis时需要通过AUTH <password>命令提供密码，默认关闭  requirepass foobared
            - 设置同一时间最大客户端连接数，默认无限制，Redis可以同时打开的客户端连接数为Redis进程可以打开的最大文件描述符数，如果设置 maxclients 0，表示不作限制。当客户端连接数到达限制时，Redis会关闭新的连接并向客户端返回max number of clients reached错误信息  maxclients 128
            - 指定Redis最大内存限制，Redis在启动时会把数据加载到内存中，达到最大内存后，Redis会先尝试清除已到期或即将到期的Key，当此方法处理 后，仍然到达最大内存设置，将无法再进行写入操作，但仍然可以进行读取操作。Redis新的vm机制，会把Key存放内存，Value会存放在swap区  maxmemory <bytes>
            - 指定是否在每次更新操作后进行日志记录，Redis在默认情况下是异步的把数据写入磁盘，如果不开启，可能会在断电时导致一段时间内的数据丢失。因为 redis本身同步数据文件是按上面save条件来同步的，所以有的数据会在一段时间内只存在于内存中。默认为no  appendonly no
            - 指定更新日志文件名，默认为appendonly.aof   appendfilename appendonly.aof
            - 指定更新日志条件，共有3个可选值：   no：表示等操作系统进行数据缓存同步到磁盘（快）   always：表示每次更新操作后手动调用fsync()将数据写到磁盘（慢，安全）   everysec：表示每秒同步一次（折衷，默认值）  appendfsync everysec
            - 指定是否启用虚拟内存机制，默认值为no，简单的介绍一下，VM机制将数据分页存放，由Redis将访问量较少的页即冷数据swap到磁盘上，访问多的页面由磁盘自动换出到内存中（在后面的文章我会仔细分析Redis的VM机制）   vm-enabled no
            - 虚拟内存文件路径，默认值为/tmp/redis.swap，不可多个Redis实例共享   vm-swap-file /tmp/redis.swap
            - 将所有大于vm-max-memory的数据存入虚拟内存,无论vm-max-memory设置多小,所有索引数据都是内存存储的(Redis的索引数据 就是keys),也就是说,当vm-max-memory设置为0的时候,其实是所有value都存在于磁盘。默认值为0   vm-max-memory 0
            - Redis swap文件分成了很多的page，一个对象可以保存在多个page上面，但一个page上不能被多个对象共享，vm-page-size是要根据存储的 数据大小来设定的，作者建议如果存储很多小对象，page大小最好设置为32或者64bytes；如果存储很大大对象，则可以使用更大的page，如果不 确定，就使用默认值   vm-page-size 3225. 设置swap文件中的page数量，由于页表（一种表示页面空闲或使用的bitmap）是在放在内存中的，，在磁盘上每8个pages将消耗1byte的内存。   vm-pages 13421772826. 设置访问swap文件的线程数,最好不要超过机器的核数,如果设置为0,那么所有对swap文件的操作都是串行的，可能会造成比较长时间的延迟。默认值为4   vm-max-threads 4
            - 设置在向客户端应答时，是否把较小的包合并为一个包发送，默认为开启  glueoutputbuf yes
            - 指定在超过一定的数量或者最大的元素超过某一临界值时，采用一种特殊的哈希算法  hash-max-zipmap-entries 64  hash-max-zipmap-value 51229. 指定是否激活重置哈希，默认为开启（后面在介绍Redis的哈希算法时具体介绍）  activerehashing yes
            - 指定包含其它的配置文件，可以在同一主机上多个Redis实例之间使用同一份配置文件，而同时各个实例又拥有自己的特定配置文件  include /path/to/local.conf










