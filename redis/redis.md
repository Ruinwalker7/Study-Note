# Redis

Redis 的主要作用是作为 Cache ，但它也不是万能的，简单总结就是数据量太大、数据访问频率非常低的业务都不适合使用 Redis，数据太大会增加成本，访问频率太低，保存在内存中纯属浪费资源。

**选择 Redis 的理由：**

- 速度快，完全基于内存，使用c语言实现，网络层使用 epoll 解决高并发问题
- 丰富的数据类型，Redis 有8种数据类型
- 代码开源，简单优雅，任何人都能吃透代码

## Redis 数据类

五种数据类型：string, hash, list, set, zset(有序集合)

### String

`string` 类型是二进制安全的，意思是 redis 可以包含任意数据，比如图片或者序列化的对象，最大存储512MB



### Hash

Redis hash 是一个键值对集合

```shell
redis 127.0.0.1:6379> HMSET runoob field1 "Hello" field2 "World"
"OK"
redis 127.0.0.1:6379> HGET runoob field1
"Hello"
redis 127.0.0.1:6379> HGET runoob field2
"World"

```

每个 hash 可以存储 $2^{32} -1$ 键值对（40多亿）。



### List

字符串列表，可以选择将新元素插入到列表的头部或者尾部。



### Set

无序集合，哈希表实现的；添加，删除，查找的复杂度都是 O(1)

sadd 命令

添加一个 string 元素到 key 对应的 set 集合中，成功返回 1，如果元素已经在集合中返回 0。

集合中最大的成员数为 232 - 1(4294967295, 每个集合可存储40多亿个成员)。

## Redis 命令

**`auth password`** ：登录

`set key value`：设置键值对

`get key`：返回key的值  

`CONFIG SET `：设置配置文件

`CONFIG GET`：获取配置文件

配置文件太多了，可以参考文档：https://juejin.cn/post/6844904032268451853

