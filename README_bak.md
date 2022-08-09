# Tips
想记录/分享后端学习过程中的知识点，能帮助到自己， 帮助到大家, 先列出比较推荐的链接供大家学习

# [算法](https://github.com/wuye251/php-interview/tree/main/doc/algorithm)
### [LeetCodeTop100](https://leetcode-cn.com/problem-list/2cktkvj/)
### [代码随想录](https://programmercarl.com/other/algo_pdf.html)

# [PHP](https://github.com/wuye251/php-interview/tree/main/doc/php)
### 基础知识
##### [empty && isset && is_null](doc/php/php-function/empty-isset-is-null.md)
##### [array_merge && +](doc/php/php-function/array-merge和+.md)

##### 魔术方法
##### 数组底层

##### interface和abstract的区别

##### php-fpm和nginx关系

##### cookie/session/token

##### php内存回收机制

#### laravel

- ##### 生命周期

- ##### 服务容器(IOC 依赖注入 --- PHP反射)

- ##### 服务提供者

- ##### 门面Facade

- ##### 契约Contract

# [mysql](https://github.com/wuye251/php-interview/tree/main/doc/mysql)
#### 事务

- 事务隔离级别
- 各个隔离界别解决的问题
- mvvc 的原理等。

#### 聚簇索引 && 非聚簇索引

#### 索引

- 包括索引的类型，索引的数据结果，b + 树和 hash 的区别，b + 树和 b 树的区别，索引创建原则。最左原则，聚簇索引，前缀索引等等。

#### 锁

- 行锁，表锁，悲观锁啊，乐观锁啊，排它锁，共享锁，间隙锁，范围锁，临键锁，两阶段锁，死锁的原因，事务中各个隔离级别用的锁的类型已经怎么用的锁。

#### 数据类型

- mysql 有哪些数据类型，varchar 与 char 的区别。int 和 int (11) 区别，tinyint 与 int 区别等等。

#### 存储引擎

#### 日志文件

- ##### redo log

- ##### undo log

- ##### bin log

#### mysql执行顺序

- ##### select是怎么执行的

- ##### update是怎么执行的

#### 注意点/优化点

#### ...

# [redis](https://github.com/wuye251/php-interview/tree/main/doc/redis)
### 数据类型

##### 使用场景
### 数据结构
### 缓存设计
##### 缓存雪崩
##### 缓存穿透
##### 缓存击穿

**缓存一致性**

- 可以看看 B 站的毛剑有次的分享

### 集群Cluster
### 持久化AOF && RDB
### REDIS为什么快
##### IO多路复用是什么 其他的类型有什么
### pipeline
### 注意点/优化点
### 附录
- [面试点](https://learnku.com/articles/61839)
- [数据结构/底层](https://mp.weixin.qq.com/s/qptE172slg_6Tl1yuzdbfw)
- [布隆过滤器](https://juejin.cn/post/6844904007790673933)

# [计算机网络](https://github.com/wuye251/php-interview/tree/main/doc/network)
### 七层模型 && 五层模型
### TCP && UDP
##### TCP 三次握手 四次挥手
###### 每次握手||挥手时 Client/Server状态
###### 为什么不能少一次握手||挥手
###### TCP为什么可靠
###### 拥塞控制
###### 慢启动
###### 快重传
###### 超时响应
###### ...

### HTTP的请求方式

##### get && post请求

##### 如何保证接口幂等性

### 附录
- [面试点](https://learnku.com/articles/59484)

# [设计模式](https://github.com/wuye251/php-interview/tree/main/doc/design-patterns)
### 单例模式
### 工厂模式
##### 简单工厂
##### 工厂
##### 抽象工厂
### ...

# 项目
### 项目经历
### 难点

# 个人评价
### 优势
### 缺点

# 反问

(以上是面试内容整理)
---------------------------------
(以下是自己准备学习的)
# docker
### 个人博客搭建在docker上
# es
### 个人博客的全局搜索 用es做
# mq
### 消息通知通过mq异步通知
# go
### 消费mq消息
# c语言
### 暂时没想到

### 附录
##### [redis面试点](https://learnku.com/articles/61839)
##### [计算机网络面试点](https://learnku.com/articles/59484)
##### [编程语言底层](https://space.bilibili.com/1154479442/video?tid=0&page=2&keyword=&order=click)
##### [redis数据结构/底层](https://mp.weixin.qq.com/s/qptE172slg_6Tl1yuzdbfw)
##### [布隆过滤器](https://juejin.cn/post/6844904007790673933)

[**go面经**](https://learnku.com/go/t/65436)

