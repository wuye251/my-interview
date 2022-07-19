> CSP: 不要通过共享内存来通信，而要通过通信来实现内存共享，它是 Go 的并发哲学，基于 channel 实现。

### chan相关命令

> 1. select{} 管道的Switch case
>
> 2. ch <- 1 发送
>
> 3. <-ch  接收
> 4. 双向管道、单向管道
> 5. WaitGroup
