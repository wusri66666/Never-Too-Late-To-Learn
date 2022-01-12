# Redis

## 一、性能测试工具redis-benchmark

~~~
redis-benchmark -h localhost -p 6379 -c 500 -n 30000
~~~

![img](Redis.assets/kuangstudye2a2d6bc-a9a0-4507-bc19-b4d6e2ab0f3b.png)

## 二、常用命令

~~~
切换数据库：select 0
清空当前数据库：flushdb
清空所有数据库：flushall
查询所有key：keys *
存值：set key value
取值：get key
查询数据库大小：dbsize
查询key是否存在（存在返回1，否则返回0）：exists key
移除key：move key 1
key设置过期时间：expire key timeout
查看key剩余存活时间：ttl key
查看key类型：type key
~~~

## 三、redis支持的数据类型

### 1、String

~~~
求value长度：strlen key
追加内容：append key "内容"
自增：
自减：
字符串范围操作：

~~~

### 2、List

~~~

~~~

### 3、Set

~~~

~~~

### 4、hash

~~~

~~~

### 5、zset

~~~

~~~

## 四、redis的事务操作







