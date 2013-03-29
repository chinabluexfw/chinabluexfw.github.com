---
layout : post
category : lessons
tags : phing phpunit
title :  phing生成phpunit报告,代码覆盖率

---

1 先配置主服务器，服务器IP192.168.0.192
	vi /etc/my.cnf或my.ini
	#在[mysqld]节点下面 确保有一下两行，并开启
	server-id=1
	log-bin=D:\data\mysql\Master\log.log 
	binlog-do-db = deercms
	 
	说明
	server-id 为主服务器的唯一标识，在主从复制架构中的每一台服务器都有一个唯一标识
	log-bin 二进制日志存放路径 
	binlog-do-db 需要复制的数据库名
	 
	2 在主服务器上创建复制账户
	GRANT REPLICATION SLAVE ON *.* TO 'test'@'129.164.152.16' IDENTIFIED BY '123456';
	账户名：test  密码：123456， 从服务器ip'129.164.152.16' 或者可以写成%，让所有IP 都可以用此账户复制，或129.164.152.%
	 
	3重启主服务器，登陆mysql,输入命令 SHOW MASTER STATUS\G  查看二进制日志情况
	+------------------+----------+--------------+------------------+
	| File             | Position | Binlog_Do_DB | Binlog_Ignore_DB |
	+------------------+----------+--------------+------------------+
	| mysql-bin.000001 | 108      |              |                  |
	+------------------+----------+--------------+------------------+
	1 row in set (0.00 sec)
	当前日志文件：mysql-bin.000001，文件位置108
 
4 从服务器（slave）端的配置
	在[mysqld]节点下面 写入下面配置
	 
	 log_bin = mysql-bin
	  #从机id，区别于主机id
	  server-id=2
	  #主机ip，供从机连接主机用
	  master-host=192.168.1.102
	  #主机端口
	  master-port=3306
	  #刚才为从机复制主机数据新建的账号
	  master-user=slave 
	  #刚才为从机复制主机数据新建的密码
	  master-password=123456
	  #连接断掉后重试间隔时间10秒
	  master-connect-retry=10 
	  #需要同步的数据库
	  replicate-do-db=deercms
	  #启用从库日志，这样可以进行链式复制,这个选项可以确保来自master 并被执行的语句会被写入slave 的二进制日志文件中(请先开启二进制日志)
	  log-slave-updates
	  #从库是否只读，0表示可读写，1表示只读
	  read-only=1
	  #二进制文件索引文件路径
	  relay-log-index=slave-relay-bin.index
	  #二进制文件路径
	  relay-log=slave-relay-bin
 
5 停止从服务器的slave
	  登陆mysql 输入以下命令
	 # 停止slave 复制服务 
	 stop slave
	 #设置从服务器的二进制配置
	  change master to master_host='localhost', master_user='test', master_password='123456',master_log_file='log.000001', master_log_pos=106;
	  master_host 主服务器ip
	  master_user 主服务器复制账户test
	  master_password 主服务器复制账户密码
	  master_log_file 主服务器目前的二进制文件名
	  master_log_pos 主服务器目前的二进制文件的当前复制位置
	 #再次开启slave
	 start slave
	#查看从服务器配置状态
	SHOW SLAVE STATUS\G
	 
	将会出现先信息
	 
	Slave_IO_State: Waiting for master to send event
	Master_Host: station192.example.com
	Master_User: tom
	Master_Port: 3306
	Connect_Retry: 60
	Master_Log_File: mysql-bin.000001
	Read_Master_Log_Pos: 175
	Relay_Log_File: mysql-relay-bin.000001
	Relay_Log_Pos: 175
	Relay_Master_Log_File: mysql-bin.000001
	Slave_IO_Running: Yes
	Slave_SQL_Running: Yes
	 
	如果输出信息有(两项必须yes) ,则说明配置成功！
	Slave_IO_Running: Yes
	Slave_SQL_Running: Yes
 
6 测试。
	


