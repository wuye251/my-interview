## 简介

字典， 又称哈希表，映射表，应用比较广泛， 以一种有意义的key-value键值对存储数据，Redis应用有以下：

- Redis的数据库， 如果string类型中 `set myString "hello world"`就是一个以myString为key ，以hello world为val的hashMap字典存储的; 
- 数据类型Hash，在键值对较多时也是用hashTable字典实现的

## Redis中字典的实现

Redis中关于字典的概念/实现有三个部分内容

- 字典哈希
- 哈希表
- 哈希表节点

下面我以大到小依次介绍Redis中的字典

### 字典哈希

Redis源码[src/dict.h](https://github.com/redis/redis/blob/7.0/src/dict.h#L79)

```c
struct dict {
    dictType *type; //类型特定函数

    dictEntry **ht_table[2]; //哈希表指向地址
    unsigned long ht_used[2]; //哈希表已使用的长度len
	
  	// reHash时的索引位置
    long rehashidx; /* rehashing not in progress if rehashidx == -1 */

    /* Keep small vars at end for optimal (minimal) struct padding */
    int16_t pauserehash; /* If >0 rehashing is paused (<0 indicates coding error) */
    signed char ht_size_exp[2]; /* exponent of size. (size = 1<<exp) */ //当前哈希表实际占用的内存长度大小
};
```

- [dictType *type](https://github.com/redis/redis/blob/7.0/src/dict.h#L63)

  ```c
  typedef struct dictType {
      uint64_t (*hashFunction)(const void *key); //计算哈希到bucket的函数
      void *(*keyDup)(dict *d, const void *key); //复制键的函数
      void *(*valDup)(dict *d, const void *obj); //复制值的函数
      int (*keyCompare)(dict *d, const void *key1, const void *key2); //键进行比较
      void (*keyDestructor)(dict *d, void *key); //销毁键的函数
      void (*valDestructor)(dict *d, void *obj); //销毁值的函数
      int (*expandAllowed)(size_t moreMem, double usedRatio); 
      /* Allow a dictEntry to carry extra caller-defined metadata.  The
       * extra memory is initialized to 0 when a dictEntry is allocated. */
      size_t (*dictEntryMetadataBytes)(dict *d);
  } dictType;
  ```

- dictEntry **ht_table[2]

  指向哈希数组存储的地址, 在Redis7.0之前的版本中，[dictht ht_table[2]](https://github.com/redis/redis/blob/6.2/src/dict.h#L83)是他的内容，在7.0之后更新成了dictEntry，具体[修改commit](https://github.com/redis/redis/commit/5e908a290ccbe9c4a7bea9356faf3b837df62793#diff-0ae7aa2e13ffeaa10fef6ab701b9261acd55e58c74023c928d6059ee5dd8d3b2L80)可以详细看下原因，commit info大概原意是合并了dictht到dict中， 优化了内存占用，使其内存占用更小

  > pr地址：https://github.com/redis/redis/pull/9228
  >
  > 有时间可以深入看下 这里先有个大概概念

- unsigned long ht_used[2]

- long rehashidx

- int16_t pauserehash

- signed char ht_size_exp[2]

### 哈希表节点

```c
typedef struct dictEntry {
    void *key; //键
  	//值
    union {
        void *val; 
        uint64_t u64;
        int64_t s64;
        double d;
    } v;
  	//哈希冲突产生的链表  指向的下一个哈希节点
    struct dictEntry *next;     /* Next entry in the same hash bucket. */
    void *metadata[];           /* An arbitrary number of bytes (starting at a
                                 * pointer-aligned address) of size as returned
                                 * by dictType's dictEntryMetadataBytes(). */
} dictEntry;
```

Redis处理哈希冲突用的是`链地址法`, 即多个key放到同一个bucket中时，会产生一条链表将这些冲突节点链接起来， 所以`next`是当前bucket节点指向下一个节点的指针

TODO: 画图



## 哈希算法

### 哈希冲突

在哈希表节点时，通过`next`我们知道对于相同bucket的多个节点，是通过链表链接起来，并用next进行向下访问的， 所以在Redis.dict出现冲突时，采用的`链地址法`， 哈希冲突处理方式也有其他如`再哈希`等, 关于冲突这块，如果不太了解的可以去看下相关教程，这里不展开说了
## 渐进式rehash

rehash的出现是因为dict会存在分配不均匀的情况，导致的情况就是

- 某个bueckt里元素很多或者哈希表中的元素很密集(会产生哈希冲突)，这对于元素多的bucket的查找就逐渐降级到了线性查找，需要进行扩展
- 对于哈希表里存储bucket较少或者为空的就比较浪费空间，需要进行收缩哈希表

负载因子是描述哈希表的稠密程度的一个指标， 当达到一个阈值时，会进行扩展/收缩

- 渐进式扩展/收缩过程

  在前面字典哈希中有dict里有`dictEntry **ht_table[2];` 就是有两个哈希表结构，那么这两个的用途就是在扩展/收缩时来用， 比如现在表数据都是在ht_table[0]中， 需要扩展/收缩长度，那么不会是立即收缩ht_table[0]的大小, 而是先扩展ht_table[1]的大小后， 逐渐把ht_table[0]的数据移动拷贝到ht_table[1]中,  在rehash时，有插入或更新,  通过`rehashidx`获取的索引写入ht_table[1]中， 保证ht_table[0]的表示减少的而不是有增加内容

> 渐进式rehash就是通过慢慢移动将ht_table[0]的数据转移到ht_table[1]， 如果有几亿条数据，一次性拷贝消耗的内存和资源太大了，也会短暂阻塞服务，所以通过rehashidx标记当前索引表的方法，实现了渐进式的扩展/收缩过程



## 参考

- 《Redis设计与实现》· 黄健宏 · 第三章 字典