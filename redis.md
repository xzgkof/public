# 设置redis访问密码 在服务器上，这里以linux服务器为例，为redis配置密码。

### 1、第一种方式 （当前这种linux配置redis密码的方法是一种临时的，如果redis重启之后密码就会失效，）

-1、首先进入redis，如果没有开启redis则需要先开启：

[root@iZ94jzcra1hZ bin]# redis-cli -p 6379

127.0.0.1:6379> 

-2、查看当前redis有没有设置密码：

127.0.0.1:6379> config get requirepass

1) "requirepass"

2) ""

-3、为以上显示说明没有密码，那么现在来设置密码：
127.0.0.1:6379> config set requirepass abcdefg

OK
127.0.0.1:6379> 

-4、再次查看当前redis就提示需要密码：
127.0.0.1:6379> config get requirepass

(error) NOAUTH Authentication required.

127.0.0.1:6379>

### 2、第二种方式 （永久方式）
需要永久配置密码的话就去redis.conf的配置文件中找到requirepass这个参数，如下配置：

修改redis.conf配置文件　　

-# requirepass foobared
requirepass 123   指定密码123 注意：前面不能有空格

保存后重启redis就可以了

 

### 连接redis

 ~~1、redis-cli连接redis~~

[root@iZ2ze3zda3caeyx6pn7c5zZ bin]# redis-cli
127.0.0.1:6379> keys *
(error) NOAUTH Authentication required.
127.0.0.1:6379> auth 123        //指定密码
OK
127.0.0.1:6379> keys *
1) "a"
2) "cit"
3) "clist"
4) "1"
127.0.0.1:6379>

 

 ~~2.Jedis连接redis~~
java 代码方式

//连接redis服务器，192.168.0.100:6379

 jedis = new Jedis("ip", 6379);
 
 ~~ 权限认证 ~~
jedis.auth("password");

 

### 配置文件方式
### xxx.xml

 <bean id=”jedisConnectionFactory”
 class=”org.springframework.data.redis.connection.jedis.JedisConnectionFactory”>
 <property name=”hostName” value=”${redis.host}” />
 <property name=”port” value=”${redis.port}” />
 <property name=”password” value=”${redis.pass}” />
 </bean>

 

 

### redis的其他命令。
如果需要关闭redis：

[root@iZ94jzcra1hZ bin]# pkill redis

如果需要开启redis：

[root@iZ94jzcra1hZ bin]# redis-server &
加&符号的作用是为了让此进程转换为后台进程，不占用shell的服务。

 

你投入得越多，就能得到越多得价值
