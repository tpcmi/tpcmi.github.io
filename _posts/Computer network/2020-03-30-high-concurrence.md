[toc]

# 高并发服务请求身份认证系统设计与实现

## 1. 线程池技术

### 1.1 概念

 线程池就是首先创建一些线程，它们的集合称为线程池。线程池在系统启动时即创建大量空闲的线程，并处于阻塞状态，不占用CPU，只占用少量内存。程序将一个任务传给线程池，线程池就会启动一条线程来执行这个任务，执行结束以后，该线程并不会死亡，而是再次返回线程池中成为空闲状态，等待执行下一个任务。

### 1.2 工作机制

- 在线程池的编程模式下，任务是提交给整个线程池，而不是直接提交给某个线程，线程池在拿到任务后，就在内部寻找是否有空闲的线程，如果有，则将任务交给某个空闲的线程。
- 一个线程同时只能执行一个任务，但可以同时向一个线程池提交多个任务。

### 1.3 特点

多线程运行时间，系统不断的启动和关闭新线程，成本非常高，会过渡消耗系统资源，并存在切换线程的危险，导致系统资源的崩溃。这时，线程池就是最好的选择了：

- 单位时间内处理任务频繁
- 对实时性要求较高

### 1.4 常见线程池

- 

- FBOS