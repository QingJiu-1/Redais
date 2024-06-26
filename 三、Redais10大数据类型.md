## 分别是哪些？
	![[Pasted image 20240623092329.png]]
	1、String（字符串）
		是Redais最基本类型，一个key对应一个value;
		类型是二进制安全的，可以是任何数据，如图片或者序列化对象；
		value最多可以是512M
	```Redais
	127.0.0.1:6379> set ke1 hello
	OK
	127.0.0.1:6379> get ke1
	"hello"
	
	```
	
	2、List（列表）
		Redais列表是最简单的字符串列表，按照插入顺序排序；
		可以添加一个元素到列表的头部或者尾部，它底层实际上是个双端链表，最多可以包含2^32 - 1
		个元素（4294967295，每个列表超过40亿个元素）
	
	 3、Hash（哈希表）
		hash是一个string类型的field（字段）和value（值）的映射表，hash特别适合用于存储对象
		每个hash也可以存储2^32 - 1 个键值对
	
	4、Set（集合）
		Set集合是通过哈希表实现的，所以添加、删除、查找的复杂度都是O(1)；
		Set是String类的无序集合。集合成员是唯一的，不会出现重复的数据，集合对象的编码可以是
		intset或者是hashtable；
		集合最大的成员数2^32 - 1
	
	 5、ZSet（有序集合）
		zset和set一样也是string类型元素的集合，且不允许重复的成员；
		不同的是每个元素都会关联一个double类型的分数，Redais正是通过分数来为集合中的成员进行
		从小到大的排序；
		zset的成员变量是唯一的，但分数（score）却可以重复；
		zset集合也是通过哈希表实现的，所以添加、删除、查询都是O(1)；
		集合最大的成员变量数为2^32 - 1
	
	6、地理空间（GEO）
		主要用于存储地理位置信息，并对存储信息进行操作，包括：
		添加地理位置的坐标；
		获取地理位置的坐标；
		计算两个位置之间的距离；
		根据用户给定的经纬度坐标来获取指定范围内的地理位置集合
	
	 7、基数统计（HyerLogLog）
		是用来做基数统计的算法，优点是：在输入元素的数量或者体积非常非常大时，计算基数所需的空
		间总是固定且是很小的；
		只需要花费12KB内存，就可以计算接近2^64个不同元素的基数；
		但它只会根据输入元素来计算基数，而不会存储输入元素本身，所以不会像集合那样，返回输入的
		各个元素。
	
	 8、位图（bitmap）
		一个字节（一个byte）= 8位；
		由很多的小格子组成，每一个格子里只能放1或0，用它来判断Y/N状态；
		由0和1状态表现的二进制位的bit数组
	
	9、位域（bitfield）
		可以一次性操作多个比特位域（指的是连续的多个比特位），它会执行一系列操作并返回一个响应
		数组，这个数组中的元素对应参数列表中的相应操作的执行结果；
		可以一次性对多个比特位域进行操作
	
	10、流（Stream）
		Redais5.0版本新增数据结构；
		主要用于消息队列（MQ），Redais本身是有一个Redais发布订阅（pub/sub）来实现消息队列的
		功能，但它有个缺点就是消息无法持久化，出现网络断开、Redais宕机等。消息就会丢失;
		发布订阅可以分发消息，但无法记录历史消息；
		而Redais Stram提供了消息的持久化和主备复制功能，可以让任何客户端访问任何时刻的数据，
		并且能记住每一个客户端的访问位置，还能保证消息不丢失。
