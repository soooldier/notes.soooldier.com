---
title: Redis
markmap:
  colorFreezeLevel: 2
---

## Persistence
### RDB
- stop-writes-on-bgsave-error `当最近一次bgsave失败时阻止写入`
- LZF 压缩算法
### AOF
#### LogRewrite流程

##### 版本 < 7.0
- 1. 创建子进程
- 2. 子进程执行rewrite操作
- 3. 父进程继续接收请求，写到旧aof文件的同时写到aof_rewrite_buffer
- 4. 子进程重写完成通知父进程
- 5. 父进程将aof_rewrite_buffer中的数据写入新的aof文件，同时rename新的aof文件
- 6. 完成

##### 版本 >= 7.0
- 1. 创建子进程
- 2. 子进程生成base aof文件
- 3. 父进程接收请求并将新的请求写入increments aof文件
- 4. 子进程完成通知父进程
- 5. 父进程利用increments aof文件与base aof文件生成临时的manifest
- 6. 使用新的manifest替换掉旧的（原子操作）
- 7. 清理旧文件，完成


## Replication
### Master节点
- replication_id `用于标识Master节点的不同数据集版本，Master节点重启或者salve晋升为master都会生成新的id`
- master_repl_offset `Master节点的复制偏移量，按命令字节数递增，没有slave的时候永远都是递增1`
### Slave节点
### 无盘复制
## 特殊数据类型
### ziplist
### liskpack