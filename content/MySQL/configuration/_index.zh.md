---
date: 2016-04-09T16:50:16+02:00
title: Configuration
weight: 20
---

MySQL 5.7 Reference Manual
MySQl binlog
binlog 就是binary log，二进制日志文件，这个文件记录了MySQL所有的DML操作。通过binlog日志我们可以做数据恢复，增量备份，主主复制和主从复制等等。
binlog开启成功之后，binlog文件的位置可以在my.inf配置文件中查看。也可以在MySQL的命令行中查看
mysqlbinlog


binlog日志，里面  
```bash
# at postion 开头 ... /*!*/； 结束
# at 77073771
#200604 14:44:09 server id 1  end_log_pos 77073836 CRC32 0x9afd36c0 	Anonymous_GTID	last_committed=5046	sequence_number=5047	rbr_only=yes
/*!50718 SET TRANSACTION ISOLATION LEVEL READ COMMITTED*//*!*/;
SET @@SESSION.GTID_NEXT= 'ANONYMOUS'/*!*/;
```
主要分为两个部分

binlog的相关概念
怎么解析binlog

binlog是记录所有数据库表结构变更（例如CREATE、ALTER TABLE…）以及表数据修改（INSERT、UPDATE、DELETE…）的二进制日志。
binlog不会记录SELECT和SHOW这类操作，因为这类操作对数据本身并没有修改，但你可以通过查询通用日志来查看MySQL执行过的所有语句。

多说一句，如果update操作没有造成数据变化，也是会记入binlog。