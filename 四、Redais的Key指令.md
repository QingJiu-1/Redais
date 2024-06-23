## 1、查询所有key
```Redais
127.0.0.1:6379> keys *
1) "k2"
2) "k3"
3) "ke1"
```

## 2、判断key是否存在
	存在显示1，可以查询多个是否存在，有几个显示几
	若不存在显示0
```Redais
127.0.0.1:6379> EXISTS k2
(integer) 1
127.0.0.1:6379> EXISTS k2 k3
(integer) 2
127.0.0.1:6379> EXISTS k1
(integer) 0
```

## 3、查询key是什么类型
```Redais
127.0.0.1:6379> TYPE k2
string
```

## 4、删除指定Key
	删除成功返回1删除没有返回0
```Redais
127.0.0.1:6379> del ke1
(integer) 1
127.0.0.1:6379> del k1
(integer) 0
```

## 5、非阻塞删除
	仅仅将keys从keyspace元数据中删除，真正的删除会在后续异步中操作
	当删除的数据量大时会造成堵塞，所以使用UNLINK
```Redais
127.0.0.1:6379> UNLINK k2
(integer) 1
```

## 6、查看还有多少秒过期
	-1表示永不过期
	-2表示已经过期
```Redais
127.0.0.1:6379> ttl k1
(integer) -1

```

## 7、为key设置过期时间
```Redais
127.0.0.1:6379> EXPIRE k1 5
(integer) 1
127.0.0.1:6379> ttl k1
(integer) 4

```

## 8、将当前数据库的key移动到给定的数据库当中
	一个Redais服务器中默认带着16个数据库，默认使用的0号数据库【0~15】
```Redais
127.0.0.1:6379> keys *
1) "k3"
2) "k1"
127.0.0.1:6379> move k3 3
(integer) 1
127.0.0.1:6379> keys *
1) "k1"

```

## 9、切换数据库
```Redais
127.0.0.1:6379> SELECT 3
OK
127.0.0.1:6379[3]> keys *
1) "k3"
```

## 10、查看当前数据库key的数量
```Redais
127.0.0.1:6379> DBSIZE
(integer) 1
127.0.0.1:6379> KEYS *
1) "k1"
```

## 11、清空当前库
```Redais
127.0.0.1:6379> KEYS *
1) "k1"
127.0.0.1:6379> FLUSHDB
OK
127.0.0.1:6379> KEYS *
(empty array)
```

## 12、通杀全部库
```Redais
127.0.0.1:6379> SELECT 3
OK
127.0.0.1:6379[3]> KEYS *
1) "k3"
127.0.0.1:6379[3]> SELECT 0
OK
127.0.0.1:6379> FLUSHALL
OK
127.0.0.1:6379> SELECT 3
OK
127.0.0.1:6379[3]> KEYS *
(empty array)
```