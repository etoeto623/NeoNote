本文以InnoDB引擎为例来说明在MySQL中一条sql语句是如何执行的。

这里涉及两个主要的日志，redo log和binlog，也就是所谓的WAL(write ahead logging)，先写日志，再写磁盘。
redo log是InnoDB实现的，redolog是MySQL的server层实现的，所有的引擎都能使用

[redo log](https://www.cnblogs.com/f-ck-need-u/archive/2018/05/08/9010872.html#auto_id_2)：文件大小是固定的，如果写入的数据超过了文件的大小，是会覆盖原来的数据的，覆盖之前，会将数据写入磁盘。**记录的是物理页的数据变化**
binlog：追加日志，不会覆盖原来的数据

redolog的结构如下：
![[redolog结构.excalidraw]]
其中，checkpoint表示当前log文件释放的位置，writepos表示日志当前写入的点，在writepos到checkpoint之间的空间是可用的

在InnoDB引擎中sql执行的流程如下：
``` plantuml
participant server层
participant innodb
participant redolog
participant binlog

server层 -> innodb: 提交sql
innodb -> innodb: 更新内存
innodb -> redolog: 写入日志，状态为prepare
innodb --> server层: 返回
server层 -> binlog: 日志写入磁盘

server层 -> innodb: 提交事务，redolog为commit
```

**[那么为什么写redolog要分为两个阶段呢？](https://www.jianshu.com/p/4bcfffb27ed5)**
这是为了保证数据的一致性，为了crash safety的考虑
假如有如下两个场景：
- 写redolog的prepare成功，写binlog失败，此时如果服务器宕机，重启后，redolog没有提交，数据都算是没有写入成功的
- 写redolog的prepare成功，写binlog成功，redolog的commit失败，此时会回滚事务
![](https://upload-images.jianshu.io/upload_images/5148507-a47b099cea71c913.png?imageMogr2/auto-orient/strip|imageView2/2/w/686/format/webp)

#redoLog    #binlog    #mysql