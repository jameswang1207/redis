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
            -  判断某个key是否存在：exists key的名字
            -  将某个移动到另外的库中：move key 2 ： 从当前库移动到2号库中
            -  给某个key设置过期时间： exprie  key 秒钟：为当前的key设置过期时间
            -  查看某个key还有多少秒过期。
            -  type key查看你的key是什么类型
        - list： 
            -  lpush|rpush|lrange : rpush  先进先出，其他的后进先出




