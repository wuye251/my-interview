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

- dictType *type

- dictEntry **ht_table[2]

  指向哈希数组存储的地址, 在Redis7.0之前的版本中，[dictht ht_table[2]](https://github.com/redis/redis/blob/6.2/src/dict.h#L83)是他的内容，在7.0之后更新成了dictEntry，具体[修改commit](https://github.com/redis/redis/commit/5e908a290ccbe9c4a7bea9356faf3b837df62793#diff-0ae7aa2e13ffeaa10fef6ab701b9261acd55e58c74023c928d6059ee5dd8d3b2L80)可以详细看下原因，commit info大概原意是合并了dictht到dict中， 优化了内存占用，使其内存占用更小

  > pr地址：https://github.com/redis/redis/pull/9228
  >
  > 有时间可以深入看下 这里先有个大概概念

- unsigned long ht_used[2]

- long rehashidx

- int16_t pauserehash

- signed char ht_size_exp[2]

### 哈希表

