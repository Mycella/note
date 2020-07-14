### 1.I/O相关概念

缓冲区操作

内核空间和用户空间

虚拟内存

分页技术

面向文件的I/O和流I/O

多工I/O（就绪性选择）

### 2.缓冲区Buffer

容量（Capacity） 容纳数据的最大量，不可被改变

上界（Limit） 第一个不能被读写的元素，现有元素的计数

位置（Position）下一个被读写的元素的索引

标记（Mark）一个备忘位置。调用 mark( )来设定 mark = postion。调用 reset( )设定 position =mark。标记在设定前是未定义的(undefined)。

四种属性的大小关系

0 <= mark <= position <= limit <= capacity

（1）所有的缓冲区都是可读的，但并不都是可写的

### 3.通道

（1）channel种类

FileChannel

SocketChannel

ServerSocketChannel

DatagramChannel



（2）文件通道不能被设置为非阻塞

（3）Scatter/Gather

（4）文件通道

文件锁依赖操作系统实现，基于文件本身而不是通道或者线程

同一个jvm中多个线程是可以打开同一个文件的，而不同的虚拟机是不能打开同一个文件的

