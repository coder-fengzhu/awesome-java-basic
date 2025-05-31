### Redis 简介
Redis（Remote Dictionary Server）是一个开源的内存数据结构存储，用作数据库、缓存和消息代理。它由 Salvatore Sanfilippo 在 2009 年创建，使用 ANSI C 编写，支持多种编程语言的客户端。Redis 提供了丰富的数据结构，包括：

+ **字符串（Strings）**：最基本的数据类型，可以存储文本或二进制数据。
+ **哈希（Hashes）**：适合存储对象。
+ **列表（Lists）**：链表结构，可以用作队列、栈等。
+ **集合（Sets）**：无序集合，支持集合操作（如交集、并集）。
+ **有序集合（Sorted Sets）**：带权重的有序集合。
+ **位图（Bitmaps）**：处理位的数组。
+ **超日志（HyperLogLogs）**：用于基数估计算法。
+ **地理空间索引（Geospatial）**：存储地理空间信息和运行半径查询。
+ **流（Streams）**：用于日志和消息流数据。

Redis 的高性能来自于其内存操作和单线程模型，确保了数据操作的原子性和一致性。

### 使用场景
Redis 的灵活性和高性能使其适用于多种应用场景：

1. **缓存**：
    - **目的**：减少数据库负载，加快数据访问速度。
    - **示例**：缓存用户会话数据、热门商品数据、计算结果等。
2. **消息队列**：
    - **目的**：实现异步处理和任务队列。
    - **示例**：任务调度、日志收集、实时消息推送等。
3. **排行榜/计数器**：
    - **目的**：高效地进行计数和排序操作。
    - **示例**：网站访问量统计、游戏排行榜、点赞数统计等。
4. **会话存储**：
    - **目的**：存储用户会话数据，实现分布式会话管理。
    - **示例**：电商网站用户购物车数据、社交媒体用户会话数据等。
5. **实时分析**：
    - **目的**：进行实时数据分析和处理。
    - **示例**：实时监控、实时日志分析、实时流数据处理等。
6. **发布/订阅系统**：
    - **目的**：实现消息的发布和订阅机制。
    - **示例**：实时聊天系统、通知推送系统等。
7. **地理空间数据**：
    - **目的**：存储和处理地理空间数据。
    - **示例**：附近商家查询、定位服务等。





### 安装和配置
#### 安装 Redis
在不同操作系统上安装 Redis 的方法有所不同。以下是一些常见的安装方法：

**Ubuntu/Debian:**

```shell
sudo apt update
sudo apt install redis-server
```

**CentOS/RHEL:**

```shell
sudo yum update
sudo yum install redis
```

**macOS (使用 Homebrew):**

```shell
brew update
brew install redis
```

****

**Windows:**

**<font style="color:rgb(51, 51, 51);">下载地址：</font>**[<font style="color:rgb(51, 51, 51);">https://github.com/dmajkic/redis/downloads</font>](https://github.com/dmajkic/redis/downloads)<font style="color:rgb(51, 51, 51);"></font>

<font style="color:rgb(51, 51, 51);">下载到的Redis支持32bit和64bit。根据自己实际情况选择，将64bit的内容cp到自定义盘符安装目录取名redis。 如 C:\redis</font>

<font style="color:rgb(51, 51, 51);">打开一个cmd窗口 使用cd命令切换目录到 C:\redis 运行</font><font style="color:rgb(51, 51, 51);"> </font>**<font style="color:rgb(51, 51, 51);">redis-server.exe redis.conf</font>**<font style="color:rgb(51, 51, 51);"> </font><font style="color:rgb(51, 51, 51);">。</font>

<font style="color:rgb(51, 51, 51);">如果想方便的话，可以把redis的路径加到系统的环境变量里，这样就省得再输路径了，后面的那个redis.conf可以省略，如果省略，会启用默认的。输入之后，会显示如下界面：</font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/26411187/1724379638229-25087e2c-f595-471a-922d-aef34525e501.jpeg)

<font style="color:rgb(51, 51, 51);">这时候另启一个cmd窗口，原来的不要关闭，不然就无法访问服务端了。</font>



#### 启动和停止 Redis
启动 Redis 服务器：

```shell
redis-server
```

连接 Redis 客户端：

```shell
redis-cli
```

停止 Redis 服务器：

```shell
redis-cli shutdown
```



### GUI访问工具
Redis Insight



### 一般命令
+ **PING**: 检测服务器是否在运行。如果服务器正常运行，将返回 "PONG"。

```shell
PING
```

+ **ECHO**: 回显输入的字符串。

```shell
ECHO "Hello, Redis!"
```

+ **SELECT**: 选择数据库（默认 0）。Redis 默认有 16 个数据库（编号从 0 到 15）。

```shell
SELECT 1
```

+ **FLUSHDB**: 清空当前数据库中的所有键。

```shell
FLUSHDB
```

+ **FLUSHALL**: 清空所有数据库中的所有键。

```shell
FLUSHALL
```

### 键操作
+ **DEL**: 删除一个或多个键。

```shell
DEL key1 key2
```

+ **EXISTS**: 检查一个键是否存在。

```shell
EXISTS key
```

+ **EXPIRE**: 设置键的过期时间（以秒为单位）。

```shell
EXPIRE key 10
```

+ **TTL**: 获取键的剩余过期时间。返回值为剩余的过期时间（秒），如果键不存在或没有设置过期时间，则返回 -2 或 -1。

```shell
TTL key
```

+ **TYPE**: 获取键的存储类型。

```shell
TYPE key
```

### 字符串（Strings）
+ **SET**: 设置键的值。

```shell
SET key value
```

+ **GET**: 获取键的值。

```shell
GET key
```

+ **INCR**: 将键的整数值加 1。

```shell
INCR key
```

+ **DECR**: 将键的整数值减 1。

```shell
DECR key
```

+ **MSET**: 同时设置多个键值对。

```shell
MSET key1 value1 key2 value2
```

+ **MGET**: 获取多个键的值。

```shell
MGET key1 key2
```

+ **SETNX**: 只有在键不存在时，才设置键的值。NX 代表 "SET if Not eXists"。

```shell
SETNX key value
```

+ **SETEX**: 设置键的值，并设置过期时间（以秒为单位）。

```shell
SETEX key seconds value
```

+ **GETSET**: 将键的值设置为新的值，并返回旧的值。

```shell
GETSET key new_value
```

### 哈希（Hashes）
+ **HSET**: 设置哈希字段的值。

```shell
HSET key field value
```

+ **HGET**: 获取哈希字段的值。

```shell
HGET key field
```

+ **HGETALL**: 获取哈希表中所有字段和值。

```shell
HGETALL key
```

+ **HDEL**: 删除一个或多个哈希字段。

```shell
HDEL key field1 field2
```

+ **HEXISTS**: 检查哈希字段是否存在。

```shell
HEXISTS key field
```

+ **HINCRBY**: 为哈希表中的字段值加上指定增量。

```shell
HINCRBY key field increment
```

+ **HSETNX**: 只有在字段不存在时，才设置哈希字段的值。NX 代表 "SET if Not eXists"。

```shell
HSETNX key field value
```

### 列表（Lists）
+ **LPUSH**: 在列表头部插入一个或多个值。

```shell
LPUSH key value1 value2
```

+ **RPUSH**: 在列表尾部插入一个或多个值。

```shell
RPUSH key value1 value2
```

+ **LPOP**: 移除并返回列表的头元素。

```shell
LPOP key
```

+ **RPOP**: 移除并返回列表的尾元素。

```shell
RPOP key
```

+ **LRANGE**: 获取列表中指定范围的元素。索引从 0 开始，负索引表示从列表末尾开始计数。

```shell
LRANGE key start stop
```

+ **LLEN**: 获取列表的长度。

```shell
LLEN key
```

+ **LREM**: 根据参数 count 的值，移除列表中与参数 value 相等的元素。

```shell
LREM key count value
```

### 集合（Sets）
+ **SADD**: 向集合添加一个或多个成员。

```shell
SADD key member1 member2
```

+ **SREM**: 移除集合中的一个或多个成员。

```shell
SREM key member1 member2
```

+ **SMEMBERS**: 返回集合中的所有成员。

```shell
SMEMBERS key
```

+ **SISMEMBER**: 检查某个值是否是集合的成员。

```shell
SISMEMBER key member
```

+ **SCARD**: 获取集合中的成员数。

```shell
SCARD key
```

+ **SUNION**: 返回给定所有集合的并集。

```shell
SUNION key1 key2
```

+ **SINTER**: 返回给定所有集合的交集。

```shell
SINTER key1 key2
```

+ **SDIFF**: 返回第一个集合与其他集合之间的差集。

```shell
SDIFF key1 key2
```

### 有序集合（Sorted Sets）
+ **ZADD**: 向有序集合添加一个或多个成员，或者更新已存在成员的分数。

```shell
ZADD key score1 member1 score2 member2
```

+ **ZRANGE**: 返回有序集合中指定区间内的成员。默认按照分数从小到大排序。

```shell
ZRANGE key start stop
```

+ **ZREM**: 移除有序集合中的一个或多个成员。

```shell
ZREM key member1 member2
```

+ **ZSCORE**: 返回有序集合中指定成员的分数。

```shell
ZSCORE key member
```

+ **ZRANGEBYSCORE**: 返回有序集合中指定分数区间内的成员。

```shell
ZRANGEBYSCORE key min max
```

+ **ZCOUNT**: 返回有序集合中指定分数区间内的成员数量。

```shell
ZCOUNT key min max
```

### 
### 发布/订阅（Pub/Sub）
+ **PUBLISH**: 将信息发送到指定的频道。

```shell
PUBLISH channel message
```

+ **SUBSCRIBE**: 订阅一个或多个频道。

```shell
SUBSCRIBE channel1 channel2
```

+ **UNSUBSCRIBE**: 退订一个或多个频道。

```shell
UNSUBSCRIBE channel1 channel2
```





#### 持久化
Redis 支持 RDB 和 AOF 两种持久化方式：

+ RDB：生成二进制快照的方式进行持久化。
+ AOF：将每个写操作记录到日志文件中进行持久化。



---



下面是一个使用 Java 与 Redis 进行交互的简单示例。这个示例使用了 Jedis，这是一个流行的 Redis Java 客户端。

### 依赖
首先，在你的项目中添加 Jedis 依赖。如果你使用的是 Maven，可以在 `pom.xml` 文件中添加以下内容：

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>4.3.1</version>
</dependency>

```

如果你使用的是 Gradle，可以在 `build.gradle` 文件中添加以下内容：

```groovy
implementation 'redis.clients:jedis:4.3.1'
```

### 示例代码
以下是一个使用 Jedis 与 Redis 进行交互的 Java 示例代码：

```java
import redis.clients.jedis.Jedis;

public class RedisExample {
    public static void main(String[] args) {
        // 连接到本地的 Redis 服务器
        Jedis jedis = new Jedis("localhost", 6379);

        // 设置一个键值对
        jedis.set("foo", "bar");

        // 获取键的值
        String value = jedis.get("foo");
        System.out.println("The value of 'foo' is: " + value);

        // 关闭连接
        jedis.close();
    }
}
```

### 运行代码
1. 确保 Redis 服务器正在运行。你可以通过命令 `redis-server` 启动 Redis 服务器。
2. 编译并运行上述 Java 代码。确保你的项目中已经包含了 Jedis 依赖。

### 说明
+ 连接 Redis：使用 `new Jedis("localhost", 6379)` 创建一个 Jedis 实例，并连接到运行在本地默认端口 6379 的 Redis 服务器。
+ 设置键值对：使用 `jedis.set("foo", "bar")` 将键 "foo" 的值设置为 "bar"。
+ 获取键的值：使用 `jedis.get("foo")` 获取键 "foo" 的值，并输出到控制台。
+ 关闭连接：使用 `jedis.close()` 关闭 Jedis 连接。

通过这种方式，你可以使用 Java 与 Redis 进行交互，完成数据存储和读取操作。如果你需要更多高级功能，比如事务、发布/订阅等，可以参考 Jedis 的文档和示例。

