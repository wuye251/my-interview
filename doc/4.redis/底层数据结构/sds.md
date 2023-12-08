## 为什么会有sds 

在Redis基本数据类型中，string是最常见也最常用的, 无论存储字符、数字、图片等或者其他复杂数据类型,   能够支持存储这么多种类型数据，仅仅沿用C语言的string是行不通的(因为以`\0`来标记字符串结尾在存储图片或者转义字符内容中如果同样包含了`\0`的内容，肯定会出错的), 故而Redis在自己实现了一个结构，支持这些复杂数据类型的存储, 那就是本文的主角`sds`

## 使用的地方

在基本数据类型string、协议内容、aof缓存、返回给客户端的回复, 都是sds，用来支撑这些复杂内容的存储

## 介绍SDS

在[redis/s rc/sds.h](https://github.com/redis/redis/blob/7.0/src/sds.h)中关于sds结构声明如下：

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

- **len**：是sds的特色之处，表示字符的长度，它让获取当前字符串长度的时间复杂度从O(n)降到了O(1)
- **alloc：**表示当前字符分配的内存空间长度，追加扩容时会进行更新
- **flags：**根据不同长度表示的不同存储类型， 如：长度<(2^7)-1时，他是sdshdr8(类型更小，占用空间更少)， 长度小于(2^16)-1时，它是更大的sdshdr16, 这样就节省了空间，关于这块类型的定义或者了解，就不展开了，这里加上[链接](https://blog.csdn.net/Mary19920410/article/details/71518130)，有兴趣的可以了解学习下
- **buf[]：**实际存储的字符串内容

## C语言中的string和Redis的SDS比较

> 以下仅介绍在Redis这种高并发场景下，SDS较C语言string的优势有哪些

1. **常数获取长度：**获取长度时复杂度为O(1), 而C语言string则需要遍历一次为O(n)
2. **二进制安全：**支持多种类型的内容存储， 就算是包含`\0`的内容也可以，因为结束以len长度来判断，而不是`\0`
3. **杜绝了缓存溢出：**这得益于有len长度的记录，在追加时判断空间的容量； C语言中则先需要手动开辟空间之后再追加，如果忘记开辟，则会造成缓存溢出的可能
4. **减少修改字符长度时造成的内存频繁分配次数：**  自己有一套分配空间的策略，尽可能减少内存分配的次数以减少系统资源的消耗
   1. 预分配：在初始化时，会预先多分配一些空间，来避免追加时再次分配的次数
   2. 惰性空间释放: 在字符串缩小时， 不会回收多余空间，避免再追加时的重新分配， 当然，底层也提供了释放这些空间的API，当你需要优化内存空间时，可以用它

## 参考

- 《Redis设计与实现》· 黄健宏 · 第二章
- [浅析C语言之uint8_t / uint16_t / uint32_t /uint64_t](https://blog.csdn.net/Mary19920410/article/details/71518130)