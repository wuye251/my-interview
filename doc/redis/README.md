# 底层数据结构

## sds

Redis中string实现没有沿用C语言中的string类型，而是用自己实现的`sds`结构，原因如下几点：

1. string类型**获取长度时需要O(n)**时间复杂度从头开始遍历一次，直到遇到`\0`结束
2. string类型中如果有`\0`那么会提前结束，产生问题，在redis.string支持多类型内容存储的情况下，不免中间会有`\0`， 这样会产生问题

在[redis/src/sds.h](https://github.com/redis/redis/blob/unstable/src/sds.h)中关于sds结构声明如下：

```c
/* Note: sdshdr5 is never used, we just access the flags byte directly.
 * However is here to document the layout of type 5 SDS strings. */
struct __attribute__ ((__packed__)) sdshdr5 {
    unsigned char flags; /* 3 lsb of type, and 5 msb of string length */
    char buf[];
};
struct __attribute__ ((__packed__)) sdshdr8 {
    uint8_t len; /* used */
    uint8_t alloc; /* excluding the header and null terminator */
    unsigned char flags; /* 3 lsb of type, 5 unused bits */
    char buf[];
};
struct __attribute__ ((__packed__)) sdshdr16 {
    uint16_t len; /* used */
    uint16_t alloc; /* excluding the header and null terminator */
    unsigned char flags; /* 3 lsb of type, 5 unused bits */
    char buf[];
};
struct __attribute__ ((__packed__)) sdshdr32 {
    uint32_t len; /* used */
    uint32_t alloc; /* excluding the header and null terminator */
    unsigned char flags; /* 3 lsb of type, 5 unused bits */
    char buf[];
};
struct __attribute__ ((__packed__)) sdshdr64 {
    uint64_t len; /* used */
    uint64_t alloc; /* excluding the header and null terminator */
    unsigned char flags; /* 3 lsb of type, 5 unused bits */
    char buf[];
};
```

（sdshdr8、sdshdr16这些还没搞明白， 不同大小  len、分配大小用不同类型表示  为优化空间？）

- **len**: 字符串真正的长度（不包含空终止字符）
- **alloc**: 字符串的最大容量，不包含 header 和最后的 '0'，初始时与 len 值一致
- **flags**: 低三位表示 header 类型
- **buf**: 柔性数组，表示一个长度动态的字符串

### 优化追加/扩容

## 双向链表

## 压缩链表

## 跳跃表

## hashTable

# 基本数据类型

## string

基于sds实现的string类型, O(1)的长度获取, 支持多种类型存储(base64图片等)

### 使用方法

- set
- get
- setex

### 使用场景

## list

- 数据少时-- 压缩链表
- 达到阈值时 -- 双向链表

### 使用方法

### 使用场景

## set

- 

### 使用方法

### 使用场景

## zset

### 使用方法

### 使用场景

## hash

- 字典HashTable
- 压缩链表

### 使用方法

### 使用场景

 

# 参考

- [【官方文档】Redis 数据类型介绍](http://www.redis.cn/topics/data-types-intro.html)

- [Redis—5种基本数据结构](https://mp.weixin.qq.com/s/MT1tB2_7f5RuOxKhuEm1vQ)
- [Redis设计与实现 - 链表](https://redisbook.readthedocs.io/en/latest/internal-datastruct/adlist.html)