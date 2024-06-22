## 日常用什么系统安装redis?
	因为在Linux系统下可以完全发挥Redais；在企业内部绝大多数也都是Linux系统下。

## Linux版安装
	安装前环境配置：
		Redais的安装必须具备gcc编译环境下进行安装；
		命令安装gcc编译环境：
		yum -y install gcc c++
		查看环境：
		gcc -v
	
	安装步骤：
		进入Redais的安装包路径下
			[root@QingJiu01 ~]# cd /opt
		查看是否存在
			[root@QingJiu01 opt]# ll
		解压安装包
			[root@QingJiu01 opt]# tar -zxvf redis-7.0.0.tar.gz 
		查看解压后的文件包名
			[root@QingJiu01 opt]# ll
		进入解压后的文件下
			[root@QingJiu01 opt]# cd redis-7.0.0/
		查看文件详情
			[root@QingJiu01 redis-7.0.0]# ll
		开始安装(采用了默认安装地址：usr/local/bin)
			[root@QingJiu01 redis-7.0.0]# make && make install
			看到Hint: It's a good idea to run 'make test' ;)这句话证明安装成功。
		查看安装好的Redais
			[root@QingJiu01 ~]# cd /usr/local/bin/
			[root@QingJiu01 bin]# pwd
			/usr/local/bin
			[root@QingJiu01 bin]# ll
			总用量 21452
			-rwxr-xr-x. 1 root root  5198208 6月  22 16:37 redis-benchmark
			lrwxrwxrwx. 1 root root       12 6月  22 16:37 redis-check-aof -> 
			redis-server
			lrwxrwxrwx. 1 root root       12 6月  22 16:37 redis-check-rdb -> 
			redis-server
			-rwxr-xr-x. 1 root root  5410960 6月  22 16:37 redis-cli
			lrwxrwxrwx. 1 root root       12 6月  22 16:37 redis-sentinel -> redis-
			server
			-rwxr-xr-x. 1 root root 11347128 6月  22 16:37 redis-server

## Redis工具功能介绍：
	redis-benchmark：性能测试工具
	redis-check-aof：修复有问题的AOF文件
	redis-check-dump：修复有问题的dump.rdb文件
	redis-cli：客户端，操作入口
	redis-sentinel：Redis集群使用
	redis-server：Redis服务器启动命令

## Redis服务启动和连接
	1、将默认的redis.conf拷贝到自己定义好的一个路径下
	2、修改配置文件
		daemonize yes 作为服务器后端启动
		protected-mode no 安全模式关闭，方便后面别的机器连接
		bind 127.0.0.1 -::1 将其注释掉，就不会限制本地ip，可以远程访问
		requirepass 123456 设置Redis自己的密码
	3、启动服务
		[root@QingJiu01 myredis]# redis-server /myredis/redis.conf 
		[root@QingJiu01 myredis]# ps -ef|grep redis|grep -v grep
		root      60779      1  0 17:14 ?        00:00:00 redis-server *:6379
	4、连接服务
		[root@QingJiu01 myredis]# redis-cli -a 123456 -p 6379
		Warning: Using a password with '-a' or '-u' option on the command line 
		interface may not be safe.
		127.0.0.1:6379>
		
		[root@QingJiu01 ~]# ps -ef|grep redis
		root      60779      1  0 17:14 ?        00:00:00 redis-server *:6379
		root      60926   2655  0 17:16 pts/1    00:00:00 redis-cli -a 123456 -p 
		6379
		root      61091  60993  0 17:18 pts/0    00:00:00 grep --color=auto redis
		
		安装成功可以正常使用
		127.0.0.1:6379> ping 
		PONG
		127.0.0.1:6379> 
	5、写入和读取
		[root@QingJiu01 myredis]# redis-cli -a 123456 -p 6379
		Warning: Using a password with '-a' or '-u' option on the command line 
		interface may not be safe.
		127.0.0.1:6379> ping
		PONG
		127.0.0.1:6379> set k1 hellowrold
		OK
		127.0.0.1:6379> get k1
		"hellowrold"
		127.0.0.1:6379> 
	6、关闭并退出
		内部关闭：
			127.0.0.1:6379> SHUTDOWN
			not connected> QUIT
			[root@QingJiu01 myredis]# lsof -i:6379
		外部关闭：
			[root@QingJiu01 ~]# redis-cli -a 123456 shutdown
			Warning: Using a password with '-a' or '-u' option on the command line 
			interface may not be safe.
			[root@QingJiu01 ~]# redis-cli -a 123456
			Warning: Using a password with '-a' or '-u' option on the command line 
			interface may not be safe.
			Could not connect to Redis at 127.0.0.1:6379: Connection refused
			not connected> ping
			Could not connect to Redis at 127.0.0.1:6379: Connection refused
			not connected> 





