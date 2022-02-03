>MySQL中的锁有**全局锁、表锁、行锁**
# 全局锁
全局锁有两种加锁的方式：
- 使用命令flush tables with read lock(FTWRL)
- set global readonly=true
推荐使用FTWRL的方式

全局锁的作用之一是在进行数据库备份时，保证数据的一致性
当然，可以使用MVCC来保证数据库备份的数据一致性（使用命令`mysqldump –single-transaction`），但并不是所有的引擎都支持MVCC
# 表级锁
表锁也分为几种：
- 表锁
- 元数据锁meta data lock(MDL)
在对表进行增删改查时，会获取MDL的读锁，对表进行结构变更时，会获取MDL的写锁

所以这里有点要注意，如果对一个表进行结构变更时，可能导致表被锁住
# 行锁
InnoDB引擎支持行锁，但使用行锁需要注意死锁的问题。
为了预防死锁，有两种方法：
- 设置锁的超时时间，通过参数`innodb_lock_wait_timeout`来设置，默认50s
- 设置死锁检测，设置参数`innodb_deadlock_detect`为`on`，但死锁检测比较耗cpu，假如同时有100个链接更新同一条数据，会死锁检测10000次，造成cpu使用率过高
``` text
每当一个事务被锁的时候，就要看看它所依赖的线程有没有被别人锁住，如此循环，最后判断是否出现了循环等待，也就是死锁。
```

#mysql    #锁