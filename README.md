# Redis笔记

* [一、大型网站的系统特点](#一大型网站的系统特点)
  * [1.高并发、大流量](#1高并发大流量)
  * [2.高可用](#2高可用)
  * [3.海量数据](#3海量数据)
  * [4.用户分布广泛，网络情况复杂](#4用户分布广泛网络情况复杂)
  * [5.安全环境恶劣](#5安全环境恶劣)
  * [6.需求快速变更，发布频繁](#6需求快速变更发布频繁)
  * [7.渐进式发展](#7渐进式发展)
* [二、大型网站架构发展历程](#二大型网站架构发展历程)
  * [1.单一应用架构](#1单一应用架构)
  * [2.垂直应用架构](#2垂直应用架构)
  * [3.分布式服务架构](#3分布式服务架构)
  * [4.流动计算架构](#4流动计算架构)
* [三、从NoSQL说起](#三从nosql说起)
* [四、Redis简介](#四redis简介)
  * [1、基本信息](#1基本信息)
  * [2、Redis的应用场景](#2redis的应用场景)
    * [①Redis的典型应用场景：](#redis的典型应用场景)
      * [[1]缓存](#1缓存)
      * [[2]数据临时存储位置](#2数据临时存储位置)
      * [[3]分布式环境下解决Session不一致问题时的Session库](#3分布式环境下解决session不一致问题时的session库)
      * [[4]流式数据去重](#4流式数据去重)
    * [②Redis不适用的场景](#redis不适用的场景)
      * [[1]直接查询value](#1直接查询value)
      * [[2]用多键一值表示特定关系](#2用多键一值表示特定关系)
      * [[3]事务中回滚](#3事务中回滚)
* [五、Redis安装](#五redis安装)
  * [1.上传并解压](#1上传并解压)
  * [2.安装C语言编译环境](#2安装c语言编译环境)
  * [3.修改安装位置](#3修改安装位置)
  * [4.编译安装](#4编译安装)
  * [5.启动Redis服务器](#5启动redis服务器)
    * [①默认启动](#默认启动)
    * [②定制配置项启动](#定制配置项启动)
      * [[1]准备配置文件](#1准备配置文件)
      * [[2]修改配置项](#2修改配置项)
      * [[3]让Redis根据指定的配置文件启动](#3让redis根据指定的配置文件启动)
  * [6.客户端登录](#6客户端登录)
* [六、Redis常用数据结构](#六redis常用数据结构)
  * [1.总体结构](#1总体结构)
  * [2.string类型](#2string类型)
  * [3.list类型](#3list类型)
  * [4.set类型](#4set类型)
  * [5.hash类型](#5hash类型)
  * [6.zset类型](#6zset类型)
  * [7.Geospatial](#7geospatial)
  * [8.HyperLogLogs](#8hyperloglogs)
  * [9.bitmap](#9bitmap)
  * [10.常用数据类型应用场景](#10常用数据类型应用场景)
* [七、Redis命令行操作](#七redis命令行操作)
  * [1.Redis命令的小套路](#1redis命令的小套路)
  * [2.基本操作](#2基本操作)
    * [①切换数据库](#切换数据库)
    * [②查看数据库长度](#查看数据库长度)
    * [③清空全库](#清空全库)
  * [3.KEY操作](#3key操作)
  * [4.string操作](#4string操作)
  * [5.list操作](#5list操作)
  * [6.set操作](#6set操作)
  * [7.hash操作](#7hash操作)
  * [8.zset操作](#8zset操作)
  * [9.Geospatial](#9geospatial)
    * [①添加地理位置](#添加地理位置)
    * [②查询已添加的地理位置](#查询已添加的地理位置)
    * [③删除已添加的地理位置](#删除已添加的地理位置)
    * [④获取指定地区的坐标值](#获取指定地区的坐标值)
    * [⑤计算两地之间的直线距离](#计算两地之间的直线距离)
    * [⑥以给定坐标为中心，在指定半径内查找元素](#以给定坐标为中心在指定半径内查找元素)
    * [⑦在指定元素周围查找其他元素](#在指定元素周围查找其他元素)
  * [10.hyperloglogs](#10hyperloglogs)
    * [①基数概念](#基数概念)
    * [②hyperloglogs命令](#hyperloglogs命令)
      * [[1]添加](#1添加)
      * [[2]统计](#2统计)
      * [[3]合并](#3合并)
  * [11.bitmap位图](#11bitmap位图)
* [八、Redis持久化机制](#八redis持久化机制)
  * [1.RDB](#1rdb)
    * [①机制描述](#机制描述)
    * [②触发时机](#触发时机)
      * [[1]基于默认配置](#1基于默认配置)
      * [[2]使用保存命令](#2使用保存命令)
      * [[3]使用flushall命令](#3使用flushall命令)
      * [[4]服务器关闭](#4服务器关闭)
    * [③相关配置](#相关配置)
    * [④思考](#思考)
  * [2.AOF](#2aof)
    * [①机制描述](#机制描述-1)
    * [②AOF基本配置](#aof基本配置)
    * [③AOF重写](#aof重写)
  * [3.持久化文件损坏修复](#3持久化文件损坏修复)
  * [4.扩展阅读：两种持久化机制的取舍](#4扩展阅读两种持久化机制的取舍)
    * [①RDB](#rdb)
      * [[1]优势](#1优势)
      * [[2]劣势](#2劣势)
    * [②AOF](#aof)
      * [[1]优势](#1优势-1)
      * [[2]劣势](#2劣势-1)
    * [③RDB和AOF并存](#rdb和aof并存)
    * [④使用建议](#使用建议)
* [九、Redis事务控制](#九redis事务控制)
  * [1.Redis事务控制的相关命令](#1redis事务控制的相关命令)
  * [2.命令队列执行失败的两种情况](#2命令队列执行失败的两种情况)
    * [①加入队列时失败](#加入队列时失败)
    * [②执行队列时失败](#执行队列时失败)
    * [③Redis为什么不支持回滚](#redis为什么不支持回滚)
  * [3.悲观锁和乐观锁](#3悲观锁和乐观锁)
* [十、Redis主从复制机制](#十redis主从复制机制)
  * [1.读写分离的好处：](#1读写分离的好处)
  * [2.搭建步骤](#2搭建步骤)
    * [①思路](#思路)
    * [②步骤](#步骤)
    * [③启动Redis主从复制集群](#启动redis主从复制集群)
  * [3.主从关系](#3主从关系)
    * [①查看主从关系](#查看主从关系)
    * [②设定主从关系](#设定主从关系)
    * [③取消主从关系](#取消主从关系)
  * [4.初步测试](#4初步测试)
  * [5.哨兵模式](#5哨兵模式)
    * [①作用](#作用)
    * [②相关概念](#相关概念)
      * [[1]主观下线](#1主观下线)
      * [[2]客观下线](#2客观下线)
      * [[3]心跳检查](#3心跳检查)
    * [③配置方式](#配置方式)
    * [④启动哨兵](#启动哨兵)
* [十一、发布订阅](#十一发布订阅)
  * [1.订阅一个频道](#1订阅一个频道)
  * [2.在一个频道上发布信息](#2在一个频道上发布信息)
* [十二、Jedis](#十二jedis)
  * [1.一个对比](#1一个对比)
  * [2.Redis准备](#2redis准备)
    * [①理解Redis配置文件中bind配置项含义](#理解redis配置文件中bind配置项含义)
    * [②查看Linux系统本机IP](#查看linux系统本机ip)
    * [③将Redis配置文件中的bind配置项设置为本机IP。](#将redis配置文件中的bind配置项设置为本机ip)
  * [3.Jedis](#3jedis)
  * [4.JedisPool](#4jedispool)
* [十三、Spring Boot Data Redis](#十三spring-boot-data-redis)
  * [1.介绍](#1介绍)
  * [2.使用方式](#2使用方式)
        * [①环境搭建](#环境搭建)
        * [②操作String类型数据](#操作string类型数据)
        * [③操作Hash类型数据](#操作hash类型数据)
        * [④操作List有序可重复集合类型数据](#操作list有序可重复集合类型数据)
        * [⑤操作Set无序不重复集合类型数据](#操作set无序不重复集合类型数据)
        * [⑥操作ZSet权重排序不重复集合类型数据](#操作zset权重排序不重复集合类型数据)
        * [⑦Redis中的通用操作](#redis中的通用操作)
* [十四、总结](#十四总结)

# 一、大型网站的系统特点

## 1.高并发、大流量

大型网站系统需要面对高并发用户，大流量访问。Google日均PV数35亿，日均IP访问数3亿；腾讯QQ的最大在线用户数1.4亿（2011年数据）；微信用户量已超11亿；2019年天猫双十一交易额突破2500亿。

## 2.高可用

系统7×24小时不间断服务。大型互联网站的宕机事件通常会成为新闻焦点，例如2010年百度域名被黑客劫持导致不能访问，成为重大新闻热点。

## 3.海量数据

需要存储、管理海量数据，需要使用大量服务器。Facebook每周上传的照片数目接近10亿，百度收录的网页数目有数百亿，Google有近百万台服务器为全球用户提供服务。

## 4.用户分布广泛，网络情况复杂

许多大型互联网都是为全球用户提供服务的，用户分布范围广，各地网络情况千差万别。在国内，还有各个运营商网络互通难的问题。而中美光缆的数次故障，也让一些对国外用户依赖较大的网站不得不考虑在海外建立数据中心。

## 5.安全环境恶劣

由于互联网的开放性，使得互联网站更容易受到攻击，大型网站几 乎每天都会被黑客攻击。任何系统漏洞都会被攻击者利用，造成重大损失。

## 6.需求快速变更，发布频繁

和传统软件的版本发布频率不同，互联网产品为快速适应市场，满足用户需求，其产品发布频率是极高的。Office的产品版本以年为单位发布，而一般大型网站的产品每周都有新版本发布上线，至于中小型网站的发布就更频繁了，有时候一天会发布几十次。

## 7.渐进式发展

与传统软件产品或企业应用系统一开始就规划好全部的功能和非功能需求不同，几乎所有的大型互联网站都是从一个小网站开始，渐进地发展起来的。 

Facebook是伯克扎克同学在哈佛大学的宿舍里开发的；Google的第一台服务器部署在斯坦福大学的实验室里；

阿里巴巴则是在马云家的客厅里诞生的。好的互联网产品都是慢慢运营出来的，不是一开始就开发好的，这也正好与网站架构的发展演化过程对应。

# 二、大型网站架构发展历程

![architecture](./assets/dubbo-architecture-roadmap.jpg)

## 1.单一应用架构

当网站流量很小时，只需一个应用，将所有功能都部署在一起，以减少部署节点和成本。此时，用于简化增删改查工作量的数据访问框架(ORM)是关键。

## 2.垂直应用架构

当访问量逐渐增大，单一应用增加机器带来的加速度越来越小，提升效率的方法之一是将应用拆成互不相干的几个应用，以提升效率。此时，用于加速前端页面开发的Web框架(MVC)是关键。

## 3.分布式服务架构

当垂直应用越来越多，应用之间交互不可避免，将核心业务抽取出来，作为独立的服务，逐渐形成稳定的服务中心，使前端应用能更快速的响应多变的市场需求。此时，用于提高业务复用及整合的分布式服务框架(RPC)是关键。

## 4.流动计算架构

当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加一个调度中心基于访问压力实时管理集群容量，提高集群利用率。此时，用于提高机器利用率的资源调度和治理中心(SOA)是关键。

# 三、从NoSQL说起

NoSQL是Not only SQL的缩写，大意为“不只是SQL”，说明这项技术是<b><font color="red">传统关系型数据库的补充</font></b>而非替代。在整个NoSQL技术栈中<b><font color="blue">MemCache</font></b>、<b><font color="blue">Redis</font></b>、<b><font color="blue">MongoDB</font></b>被称为NoSQL三剑客。那么时代为什么需要NoSQL数据库呢？我们来做个对比：

|              | 关系型数据库             | NoSQL数据库                            |
| ------------ | ------------------------ | -------------------------------------- |
| 数据存储位置 | 硬盘                     | 内存                                   |
| 数据结构     | 高度组织化结构化数据     | 没有预定义的模式                       |
| 数据操作方式 | SQL                      | 所有数据都是键值对，没有声明性查询语言 |
| 事务控制     | 严格的基础事务ACID原则   | 基于乐观锁的松散事务控制               |
| 访问控制     | 细粒度的用户访问权限控制 | 简单的基于IP绑定或密码的访问控制       |
| 外键         | 支持                     | 不支持                                 |
| 索引         | 支持                     | 不支持                                 |

所以NoSQL数据库的最大优势体现为：高性能、高可用性和可伸缩性。

# 四、Redis简介

## 1、基本信息

Redis英文官网介绍：

``` html
Redis is an open source (BSD licensed), in-memory data structure store, used as a database, cache and message broker. It supports data structures such as strings, hashes, lists, sets, sorted sets with range queries, bitmaps, hyperloglogs and geospatial indexes with radius queries. Redis has built-in replication, Lua scripting, LRU eviction, transactions and different levels of on-disk persistence, and provides high availability via Redis Sentinel and automatic partitioning with Redis Cluster.
```

Redis中文官网介绍：

``` html
Redis是一个开源（BSD许可）的，内存中的数据结构存储系统，它可以用作数据库、缓存和消息中间件。 它支持多种类型的数据结构，如字符串（strings），散列（hashes），列表（lists），集合（sets），有序集合（sorted sets） 与范围查询，bitmaps，hyperloglogs和地理空间（geospatial） 索引半径查询。 Redis 内置了复制（replication），LUA脚本（Lua scripting）， LRU驱动事件（LRU eviction），事务（transactions）和不同级别的 磁盘持久化（persistence）， 并通过Redis哨兵（Sentinel）和自动分区（Cluster）提供高可用性（high availability）。
```

下面是几点补充：

- Redis的名字是Remote Dictionary Server的缩写。
- 开发语言是ANSI C。
- 支持多种不同语言的客户端。
- 官方给出的性能参考（在并发量50的情况下）：
  - GET: 110000/s:读的速度每秒11万次
  - SET: 81000/s: 写的速度每秒8万次

中文官网http://www.redis.cn<br/>
英文官网http://redis.io<br/>

Redis命令参考文档网址：http://redisdoc.com<br/>

## 2、Redis的应用场景

### ①Redis的典型应用场景：

#### [1]缓存

使用Redis可以建立性能非常出色的缓存服务器，查询请求先在Redis中查找所需要的数据，如果能够查询到（命中）则直接返回，大大减轻关系型数据库的压力。

#### [2]数据临时存储位置

使用token（令牌）作为用户登录系统时的身份标识，这个token就可以在Redis中临时存储。

#### [3]分布式环境下解决Session不一致问题时的Session库

Spring提供了一种技术解决分布式环境下Session不一致问题，叫SpringSession。而Redis就可以为SpringSession提供一个数据存储空间。

#### [4]流式数据去重

在Redis中有一种数据类型是set，和Java中的Set集合很像，不允许存储重复数据。借助这个特性我们可以在Redis中使用set类型存储流式数据达到去重的目的。

### ②Redis不适用的场景

#### [1]直接查询value

Redis是一个严格的Key-value数据库，所有数据都必须通过key去找到value，Redis没有提供直接根据查询条件匹配value的方法。

#### [2]用多键一值表示特定关系

Redis中一个key对应一个value，没有多个key对应同一个value的情况。

#### [3]事务中回滚

Redis不支持回滚。如果一个命令在加入队列时没有检测出问题，那么队列执行时不会因为某一条命令失败而回滚。

# 五、Redis安装

## 1.上传并解压

redis-4.0.2.tar.gz

## 2.安装C语言编译环境

[建议先拍快照]<br/>

yum install -y gcc-c++

> 如果不能联网，可以使用下面步骤安装：
>
> 1.上传gcc-c++.rpm.packages目录到Linux系统
> 2.拍摄快照
> 3.进入rpm包所在目录
> 4.执行安装
> rpm -Uvh *.rpm --nodeps --force
> 5.验证安装效果
> gcc -v

## 3.修改安装位置

vim redis解压目录/src/Makefile

``` html
PREFIX?=/usr/local/redis
```

就Redis自身而言是不需要修改的，这里修改的目的是让Redis的运行程序不要和其他文件混杂在一起。

## 4.编译安装

编译：进入Redis解压目录执行make命令

[建议先拍快照]

安装：make install

## 5.启动Redis服务器

### ①默认启动

``` html
[root@rich ~]# /usr/local/redis/bin/redis-server
7239:C 07 Oct 18:59:12.144 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
7239:C 07 Oct 18:59:12.144 # Redis version=4.0.2, bits=64, commit=00000000, modified=0, pid=7239, just started
7239:C 07 Oct 18:59:12.144 # Warning: no config file specified, using the default config. In order to specify a config file use /usr/local/redis/bin/redis-server /path/to/redis.conf
7239:M 07 Oct 18:59:12.145 * Increased maximum number of open files to 10032 (it was originally set to 1024).
                _._                                                  
           _.-``__ ''-._                                             
      _.-``    `.  `_.  ''-._           Redis 4.0.2 (00000000/0) 64 bit
  .-`` .-```.  ```\/    _.,_ ''-._                                   
 (    '      ,       .-`  | `,    )     Running in standalone mode
 |`-._`-...-` __...-.``-._|'` _.-'|     Port: 6379
 |    `-._   `._    /     _.-'    |     PID: 7239
  `-._    `-._  `-./  _.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |           http://redis.io        
  `-._    `-._`-.__.-'_.-'    _.-'                                   
 |`-._`-._    `-.__.-'    _.-'_.-'|                                  
 |    `-._`-._        _.-'_.-'    |                                  
  `-._    `-._`-.__.-'_.-'    _.-'                                   
      `-._    `-.__.-'    _.-'                                       
          `-._        _.-'                                           
              `-.__.-'                                               

7239:M 07 Oct 18:59:12.148 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
7239:M 07 Oct 18:59:12.148 # Server initialized
7239:M 07 Oct 18:59:12.148 # WARNING overcommit_memory is set to 0! Background save may fail under low memory condition. To fix this issue add 'vm.overcommit_memory = 1' to /etc/sysctl.conf and then reboot or run the command 'sysctl vm.overcommit_memory=1' for this to take effect.
7239:M 07 Oct 18:59:12.148 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
7239:M 07 Oct 18:59:12.148 * Ready to accept connections
```

停止Redis服务器可以在另外一个命令行窗口执行下面命令：

``` html
/usr/local/redis/bin/redis-cli shutdown
```

``` html
7239:M 07 Oct 19:00:53.208 # User requested shutdown...
7239:M 07 Oct 19:00:53.208 * Saving the final RDB snapshot before exiting.
7239:M 07 Oct 19:00:53.214 * DB saved on disk
7239:M 07 Oct 19:00:53.214 # Redis is now ready to exit, bye bye...
```

### ②定制配置项启动

#### [1]准备配置文件

cp redis解压目录/redis.conf /usr/local/redis/

#### [2]修改配置项

| 配置项名称 | 作用                                  | 取值                  |
| ---------- | ------------------------------------- | --------------------- |
| daemonize  | 控制是否以守护进程形式运行Redis服务器 | yes                   |
| logfile    | 指定日志文件位置                      | "/var/logs/redis.log" |
| dir        | Redis工作目录                         | /usr/local/redis      |

**注意：/var/logs目录需要我们提前创建好**

#### [3]让Redis根据指定的配置文件启动

格式

``` html
redis-server文件路径 redis.conf文件路径
```

举例

``` html
/usr/local/redis/bin/redis-server /usr/local/redis/redis.conf
```

## 6.客户端登录

```
/usr/local/redis/bin/redis-cli
```

``` html
	127.0.0.1:6379> ping
	PONG
	127.0.0.1:6379> exit
```

# 六、Redis常用数据结构

## 1.总体结构

<table>
    <tr>
    	<th>KEY</th>
        <th>VALUE</th>
        <th>相当于Java中</th>
    </tr>
    <tr>
        <td rowspan="5">string</td>
    	<td>string</td>
        <td>Map&lt;String,String&gt;</td>
    </tr>
    <tr>
    	<td>list</td>
        <td>Map&lt;String,List&lt;String&gt;&gt;</td>
    </tr>
    <tr>
    	<td>set</td>
        <td>Map&lt;String,Set&lt;String&gt;&gt;</td>
    </tr>
    <tr>
    	<td>hash</td>
        <td>Map&lt;String,Map&lt;String,String&gt;&gt;</td>
    </tr>
    <tr>
    	<td>zset</td>
        <td>Map&lt;String,ScoreMember&gt;<br><br>class&nbsp;ScoreMember{<br>&nbsp;&nbsp;&nbsp;&nbsp;private&nbsp;Double&nbsp;score;<br>&nbsp;&nbsp;&nbsp;&nbsp;private&nbsp;String&nbsp;member;<br>}</td>
    </tr>
</table>

Redis中的数据，总体上是键值对，不同数据类型指的是键值对中值的类型。

## 2.string类型

Redis中最基本的类型，它是key对应的一个单一值。二进制安全，不必担心由于编码等问题导致二进制数据变化。所以redis的string可以包含任何数据，比如jpg图片或者序列化的对象。Redis中一个字符串值的最大容量是512M。

## 3.list类型

Redis 列表是简单的字符串列表，按照插入顺序排序。你可以添加一个元素到列表的头部（左边）或者尾部（右边）。它的底层是双向链表，所以它操作时头尾效率高，中间效率低（额外花费查找插入位置的时间）。<br/>

在Redis中list类型是按照插入顺序排序的字符串链表。和数据结构中的普通链表一样，我们可以在其头部(left)和尾部(right)添加新的元素。在插入时，如果该键并不存在，Redis将为该键创建一个新的链表。与此相反，如果链表中所有的元素均被移除，那么该键也将会被从数据库中删除。List中可以包含的最大元素数量是2^32-1个。<br/>

list是一个有序可以重复的数据类型。

![p01](./assets/p01.png)

## 4.set类型

Redis的set是string类型的无序集合。它是基于哈希表实现的。set类型插入数据时会自动去重。最大可以包含2^32-1个元素。

![p05](./assets/p06.png)

## 5.hash类型

本身就是一个键值对集合。可以当做Java中的Map<String,String>对待。每一个hash可以存储2^32-1个键值对。

![p05](./assets/p05.png)

## 6.zset类型

Redis zset 和 set 一样也是string类型元素的集合,且不允许重复的成员。不同的是每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。zset的成员是唯一的,但分数(score)却可以重复。

![p05](./assets/p07.png)

## 7.Geospatial

Redis 在 3.2 推出 Geo 类型，该功能可以推算出地理位置信息，两地之间的距离。

![./images](./assets/p08.png)

## 8.HyperLogLogs

用于大数据量基数统计，速度非常快，占用内存非常小。每个HyperLogLog键只需要花费12KB内存，就可以计算接近 2^64个不同元素的基数。比如计算网站UV（User view，用户访问数量，一个用户一天访问同一个URL地址多次合并为一次）。

## 9.bitmap

直接对string的二进制位进行操作的一组命令

## 10.常用数据类型应用场景

| 数据类型     | 应用场景                                                     |
| ------------ | ------------------------------------------------------------ |
| string       | 分布式Session存储<br />分布式数据库ID<br />计数器：统计网站访问量 |
| hash         | 存储对象信息（购物车中的商品信息）<br />存储表的信息         |
| list         | 实现队列、栈操作<br />汇总日志<br />粉丝列表<br />关注的人列表 |
| set          | 签到<br />打卡<br />点赞                                     |
| zset         | 排行榜<br />百度热点搜索                                     |
| geospatial   | 获取地理位置信息<br />两地之间的距离                         |
| hyperloglogs | 基数统计                                                     |
| bitmaps      | 统计用户访问次数                                             |

# 七、Redis命令行操作

## 1.Redis命令的小套路

- NX：not exist （不存在）
- EX：expire （设置可以存在的时长）
- M：multi （多个）

## 2.基本操作

### ①切换数据库

``` html
Redis默认有16个数据库。
	115 # Set the number of databases. The default database is DB 0, you can select
	116 # a different one on a per-connection basis using SELECT <dbid> where
	117 # dbid is a number between 0 and 'databases'-1
	118 databases 16
使用select进行切换，数据库索引从0开始
    127.0.0.1:6379> select 2
    OK
    127.0.0.1:6379[2]> select 0
    OK
    127.0.0.1:6379> 
```

### ②查看数据库长度

数据库长度就是这个数据库中存储了多少条数据

``` html
127.0.0.1:6379> dbsize
(integer) 3
```

### ③清空全库

```html
127.0.0.1:6379> flushall
```

## 3.KEY操作

在实际操作中对于Key的定义大家注意下面几点：

- Key不要太长，超过1024字节将消耗过多内存，降低查询效率。尽管Redis支持的Key最大长度为512MB。
- Key仍然要做到见名知意。
- 在同一个项目中遵循同一个命名规范，习惯上多个单词用“:”分开。例如：“user:token:session:id”
- Redis命令不区分大小写，Key区分大小写

``` html
●KEYS PATTERN
	把匹配PATTERN的key返回。PATTERN中可以使用“*”匹配多个字符，使用“?”匹配单个字符
●TYPE KEY
	返回KEY对应的值的类型
●MOVE KEY DB
	把一组键值对数据移动到另一个数据库中
●DEL KEY [KEY ...]
	根据KEY进行删除，至少要指定一个KEY
●EXISTS KEY [KEY ...]
	检查指定的KEY是否存在。指定一个KEY时，存在返回1，不存在返回0。可以指定多个，返回存在的KEY的数量。
●RENAME KEY NEWKEY
	重命名一个KEY，NEWKEY不管是否是已经存在的都会执行，如果NEWKEY已经存在则会被覆盖。
●RENAMENX KEY NEWKEY
	只有在NEWKEY不存在时能够执行成功，否则失败
●TTL KEY
	以秒为单位查看KEY还能存在多长时间
	正数：剩余的存活时间（单位：秒）
	-1：永不过期
	-2：不存在的Key
●EXPIRE KEY SECONDS
	给一个KEY设置在SECONDS秒后过期，过期会被Redis移除。
●PERSIST KEY
	移除过期时间，变成永久key
```

## 4.string操作

``` html
●SET KEY VALUE [EX SECONDS] [PX MILLISECONDS] [NX|XX]
	给KEY设置一个string类型的值。
	EX参数用于设置存活的秒数。
	PX参数用于设置存活的毫秒数。
	NX参数表示当前命令中指定的KEY不存在才行。
	XX参数表示当前命令中指定的KEY存在才行。
●GET KEY
	根据key得到值，只能用于string类型。
●APPEND KEY VALUE
	把指定的value追加到KEY对应的原来的值后面，返回值是追加后字符串长度
●STRLEN KEY
	直接返回字符串长度
●INCR KEY
	自增1（要求：参与运算的数据必须是整数且不能超过整数Integer范围）
●DECR KEY
	自减1（要求：参与运算的数据必须是整数且不能超过整数Integer范围）
●INCRBY KEY INCREMENT
	原值+INCREMENT（要求：参与运算的数据必须是整数且不能超过整数Integer范围）
●DECRBY KEY DECREMENT
	原值-DECREMENT（要求：参与运算的数据必须是整数且不能超过整数Integer范围）
●GETRANGE KEY START END
	从字符串中取指定的一段，索引从0开始
	START是开始取值的索引
	END是结束取值的索引
●SETRANGE KEY OFFSET VALUE
	从offset（从0开始的索引）开始使用VALUE进行替换
	包含offset位置
●SETEX KEY SECONDS VALUE
	设置KEY,VALUE时指定存在秒数
●SETNX KEY VALUE
	新建字符串类型的键值对
●MSET KEY VALUE [KEY VALUE ...]
	一次性设置一组多个键值对
●MGET KEY [KEY ...]
	一次性指定多个KEY，返回它们对应的值，没有值的KEY返回值是(nil)
●MSETNX KEY VALUE [KEY VALUE ...]
	一次性新建多个值
●GETSET KEY VALUE
	设置新值，同时能够将旧值返回

```

## 5.list操作

``` html
●LPUSH key value [value ...]
	针对key指定的list，从左边放入元素
●RPUSH key value [value ...]
	针对key指定的list，从右边放入元素
●LRANGE key start stop
	根据list集合的索引打印元素数据
	正着数：0,1,2,3,...
	倒着数：-1,-2,-3,...
●LLEN key
	返回list集合的长度
●LPOP key
	从左边弹出一个元素。
	弹出=返回+删除。
●RPOP key
	从右边弹出一个元素。
●RPOPLPUSH source destination
	从source中RPOP一个元素，LPUSH到destination中
●LINDEX key index
	根据索引从集合中取值
●LINSERT key BEFORE|AFTER pivot value
	在pivot指定的值前面或后面插入value
	如果pivot值有重复的，那么就从左往右数，以第一个遇到的pivot为基准
	BEFORE表示放在pivot前面
	AFTER表示放在pivot后面
●LPUSHX key value
	只能针对存在的list执行LPUSH
●LREM key count value
	根据count指定的数量从key对应的list中删除value
	具体执行时从左往右删除，遇到一个删一个，删完为止
●LSET key index value
	把指定索引位置的元素替换为另一个值
●LTRIM key start stop
	仅保留指定区间的数据，两边的数据被删除

```

## 6.set操作

``` html
●SADD key member [member ...]
	给key指定的set集合中存入数据，set会自动去重
●SMEMBERS key
	返回可以指定的set集合中所有的元素
●SCARD key
	返回集合中元素的数量
●SISMEMBER key member
	检查当前指定member是否是集合中的元素
	返回1：表示是集合中的元素
	返回2：表示不是集合中的元素
●SREM key member [member ...]
	从集合中删除元素
●SINTER key [key ...]
	将指定的集合进行“交集”操作
	集合A：a,b,c
	集合B：b,c,d
	交集：b,c
●SINTERSTORE destination key [key ...]
	取交集后存入destination
●SDIFF key [key ...]
	将指定的集合执行“差集”操作
	集合A：a,b,c
	集合B：b,c,d
	A对B执行diff：a
	相当于：A-交集部分
●SDIFFSTORE destination key [key ...]
●SUNION key [key ...]
	将指定的集合执行“并集”操作
	集合A：a,b,c
	集合B：b,c,d
	并集：a,b,c,d
●SUNIONSTORE destination key [key ...]
●SMOVE source destination member
	把member从source移动到destination

【测试数据
SADD testset a b c d e f g h i j k l m n o p q r s t u v w x y z aa bb cc dd ee ff gg hh ii jj kk ll mm nn oo pp qq rr ss tt uu vv ww xx yy zz
】

●SSCAN key cursor [MATCH pattern] [COUNT count]
	基于游标的遍历。cursor是游标值，第一次显示第一块内容时，游标取值为0；根据后续返回的新的游标值获取下一块数据。直到游标值变成0，说明数据遍历完成。
●SRANDMEMBER key [count]
	从集合中随机返回count个数量的元素，count不指定就返回1个（数据有可能重复出现）
●SPOP key [count]
	从集合中随机弹出count个数量的元素，count不指定就弹出1个（保证不会有重复数据出现）
```

## 7.hash操作

``` html
●HSET key field value
	插入新数据返回1
	修改旧数据返回0
●HGETALL key
●HGET key field
●HLEN key
●HKEYS key
●HVALS key
●HEXISTS key field
●HDEL key field [field ...]
●HINCRBY key field increment
●HMGET key field [field ...]
●HMSET key field value [field value ...]
●HSETNX key field value
	要求field是新建的
```

## 8.zset操作

``` html
●ZADD key [NX|XX] [CH] [INCR] score member [score member ...]
●ZRANGE key start stop [WITHSCORES]
●ZCARD key
●ZSCORE key member
●ZINCRBY key increment member
●ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT offset count]
	在分数的指定区间内返回数据
	min参数可以通过 -inf 表示负无穷
	max参数可以通过 +inf 表示正无穷

	默认是闭区间
	可以通过 (min (max 形式指定开区间，例如：(50 (80
●ZRANK key member
	先对分数进行升序排序，返回member的排名。排名从0开始
●ZREM key member [member ...]
```

## 9.Geospatial

查询经纬度数据：http://www.jsons.cn/lngcode

### ①添加地理位置

```redis
GEOADD key longitude latitude member [longitude latitude member ...]
```

> 规则：
>
> 1.两极无法直接添加，一般会下载城市数据，直接通过 Java 程序一次性导入。
>
> 2.取值范围
>
> ​	有效的经度从 -180 度到 180 度。
>
> ​	有效的纬度从 -85.05112878 度到 85.05112878 度。
>
> ​	当坐标位置超出指定范围时，该命令将会返回一个错误。
>
> 3.已经添加的数据，是无法再次往里面添加的。

```html
192.168.109.100:6379> GEOADD "china:city" 114.085947 22.547 shenzhen
(integer) 1
192.168.109.100:6379> GEOADD "china:city" 113.280637 23.125178 guangzhou
(integer) 1
```

### ②查询已添加的地理位置

Geo类型在Redis内部其实是使用zset类型存储的，所以可以使用zset的命令进行常规操作。

```redis
192.168.109.100:6379> ZRANGE china:city 0 -1 
1) "shenzhen"
2) "guangzhou"
192.168.109.100:6379> ZRANGE china:city 0 -1 WITHSCORES
1) "shenzhen"
2) "4046433733682118"
3) "guangzhou"
4) "4046533764066819"
```

### ③删除已添加的地理位置

```redis
192.168.109.100:6379> ZREM china:city guangzhou
(integer) 1
```

### ④获取指定地区的坐标值

```redis
192.168.109.100:6379> GEOPOS china:city shenzhen
1) 1) "114.08594459295272827"
   2) "22.54699993773966327"
```

### ⑤计算两地之间的直线距离

```redis
192.168.109.100:6379> GEODIST china:city guangzhou shenzhen km
"104.6426"
```

> 单位：
>
> m 表示单位为米[默认值]。
>
> km 表示单位为千米。
>
> mi 表示单位为英里。
>
> ft 表示单位为英尺。
>
> 如果用户没有显式地指定单位参数， 那么 GEODIST 默认使用米作为单位。

### ⑥以给定坐标为中心，在指定半径内查找元素

```redis
192.168.109.100:6379> GEORADIUS china:city 110 20 1000 km WITHCOORD WITHDIST
1) 1) "shenzhen"
   2) "509.4622"
   3) 1) "114.08594459295272827"
      2) "22.54699993773966327"
2) 1) "guangzhou"
   2) "485.7406"
   3) 1) "113.28063815832138062"
      2) "23.12517743834835215"
```

WITHCOORD表示显示经纬度<br/>

WITHDIST表示显示到中心的距离

### ⑦在指定元素周围查找其他元素

```redis
192.168.109.100:6379> GEORADIUSBYMEMBER china:city shenzhen 300 km WITHCOORD WITHDIST
1) 1) "shenzhen"
   2) "0.0000"
   3) 1) "114.08594459295272827"
      2) "22.54699993773966327"
2) 1) "guangzhou"
   2) "104.6426"
   3) 1) "113.28063815832138062"
      2) "23.12517743834835215"
```

## 10.hyperloglogs

### ①基数概念

一个集合中不重复元素的个数。例如：集合{1,2,5,1,7,2,5}中元素个数是7，但是基数是4。而hyperloglogs的主要功能就是进行基数统计。

### ②hyperloglogs命令

#### [1]添加

```redis
192.168.109.100:6379> PFADD user:access:1 tom jerry andy jim andy jerry tom
(integer) 1
192.168.109.100:6379> PFADD user:access:2 andy jerry tom bob kate
(integer) 1
192.168.109.100:6379> PFADD user:access:3 mary harry tom jerry
(integer) 1
```

#### [2]统计

```redis
192.168.109.100:6379> PFCOUNT user:access:1 user:access:2 user:access:3
(integer) 8
```

#### [3]合并

```redis
192.168.109.100:6379> PFMERGE user:access:merge user:access:1 user:access:2 user:access:3
OK
192.168.109.100:6379> PFCOUNT user:access:merge
(integer) 8
```

## 11.bitmap位图

直接对数据的二进制位进行操作

```redis
192.168.109.100:6379[5]> set a hello
OK
192.168.109.100:6379[5]> GETBIT a 0
(integer) 0
192.168.109.100:6379[5]> GETBIT a 1
(integer) 1
192.168.109.100:6379[5]> GETBIT a 2
(integer) 1
192.168.109.100:6379[5]> GETBIT a 3
(integer) 0
192.168.109.100:6379[5]> GETBIT a 4
(integer) 1
192.168.109.100:6379[5]> GETBIT a 5
(integer) 0
192.168.109.100:6379[5]> SETBIT a 5 1
(integer) 0
192.168.109.100:6379[5]> get a
"lello"
192.168.109.100:6379[5]> BITCOUNT a
(integer) 22
```

setbit设置指定比特位<br/>
getbit获取指定比特位<br/>
bitcount统计所有比特位中1的数量

# 八、Redis持久化机制

[官网描述](https://redis.io/topics/persistence#snapshotting)

Redis工作时数据都存储在内存中，万一服务器断电，则所有数据都会丢失。针对这种情况，Redis采用持久化机制来增强数据安全性。说白了就是把内存里的数据保存到硬盘上。

## 1.RDB

### ①机制描述

在满足特定触发条件时把内存中的<span style="color:blue;font-weight:bold;">数据本身</span>作为一个<span style="color:blue;font-weight:bold;">快照</span>保存到硬盘上的文件中。Redis默认开启RDB机制。

### ②触发时机

#### [1]基于默认配置

``` properties
save 900 1
save 300 10
save 60 10000
```

含义

| 配置          | 含义                                |
| ------------- | ----------------------------------- |
| save 900 1    | 900秒内至少有一次修改则触发保存操作 |
| save 300 10   | 300秒内至少有10次修改则触发保存操作 |
| save 60 10000 | 60秒内至少有1万次修改则触发保存操作 |

#### [2]使用保存命令

save或bgsave

#### [3]使用flushall命令

这个命令也会产生dump.rdb文件，但里面是空的，没有意义

#### [4]服务器关闭

如果执行SHUTDOWN命令让Redis正常退出，那么此前Redis就会执行一次持久化保存。

### ③相关配置

| 配置项     | 取值                   | 作用                                                         |
| ---------- | ---------------------- | ------------------------------------------------------------ |
| save       | ""                     | 禁用RDB机制                                                  |
| dbfilename | 文件名，例如：dump.rdb | 设置RDB机制下，数据存储文件的文件名                          |
| dir        | Redis工作目录路径      | 指定存放持久化文件的目录的路径。注意：这里指定的必须是目录不能是文件名 |

### ④思考

RDB 机制能够保证数据的绝对安全吗？

- 机制本身并不能保证绝对安全
- 但是作为总体机制的一部分，RDB（或AOF）机制能够起到它独特的作用；整体机制综合来看是可靠的

## 2.AOF

### ①机制描述

根据配置文件中指定的策略，把生成数据的命令保存到硬盘上的文件中。一个AOF文件的内容可以参照下面的例子：

``` properties
*2
$6
SELECT
$1
0
*3
$3
set
$3
num
$2
10
*2
$4
incr
$3
num
*2
$4
incr
$3
num
*2
$4
incr
$3
num
```

生成上面文件内容的Redis命令是：

``` html
set num 10
incr num
incr num
incr num
```

### ②AOF基本配置

| 配置项         | 取值              | 作用                                                         |
| -------------- | ----------------- | ------------------------------------------------------------ |
| appendonly     | yes               | 启用AOF持久化机制                                            |
|                | no                | 禁用AOF持久化机制[默认值]                                    |
| appendfilename | "文件名"          | AOF持久化文件名                                              |
| dir            | Redis工作目录路径 | 指定存放持久化文件的目录的路径。注意：这里指定的必须是目录不能是文件名 |
| appendfsync    | always            | 每一次数据修改后都将执行文件写入操作，是最安全的方式但是速度缓慢。 |
|                | everysec          | 每秒执行一次写入操作。折中。                                 |
|                | no                | 由操作系统在适当的时候执行写入操作，Redis性能最好，数据保存次数最少。 |

**warning**

当 AOF 和 RDB 机制并存时，Redis 会优先采纳 AOF 机制。使用 AOF 持久化文件恢复内存中的数据。而 AOF 刚刚开启时 appendonly.aof 持久化文件中没有任何数据。拿空的 appendonly.aof 持久化文件恢复内存，就会导致以前所有数据都丢失。

### ③AOF重写

对比下面两组命令：

| AOF重写前                                                   | AOF重写后   |
| ----------------------------------------------------------- | ----------- |
| set count 1<br />incr count<br />incr count<br />incr count | set count 4 |

两组命令执行后对于count来说最终的值是一致的，但是进行AOF重写后省略了中间过程，可以让AOF文件体积更小，缩短数据恢复时间。而Redis会根据AOF文件的体积来决定是否进行AOF重写。参考的配置项如下：

| 配置项                          | 含义                            |
| ------------------------------- | ------------------------------- |
| auto-aof-rewrite-percentage 100 | 文件体积增大100%时执行AOF重写   |
| auto-aof-rewrite-min-size 64mb  | 文件体积增长到64mb时执行AOF重写 |

实际工作中不要进行频繁的AOF重写，因为CPU、内存资源和硬盘资源二者之间肯定是CPU、内存资源更加宝贵，所以不应该过多耗费CPU性能去节省硬盘空间。另外数据恢复也不是高频操作，所以节约数据恢复时间价值也不是非常大。

## 3.持久化文件损坏修复

Redis服务器启动时如果读取了损坏的持久化文件会导致启动失败，此时为了让Redis服务器能够正常启动，需要对损坏的持久化文件进行修复。这里以AOF文件为例介绍修复操作的步骤。

- 第一步：备份要修复的appendonly.aof文件

- 第二步：执行修复程序

  /usr/local/redis/bin/redis-check-aof --fix /usr/local/redis/appendonly.aof

- 第三步：重启Redis

注意：所谓修复持久化文件仅仅是把损坏的部分去掉，而没法把受损的数据找回。

## 4.扩展阅读：两种持久化机制的取舍

### ①RDB

#### [1]优势

适合大规模的数据恢复，速度较快

#### [2]劣势

会丢失最后一次快照后的所有修改，不能绝对保证数据的高度一致性和完整性。Fork的时候，内存中的数据被克隆了一份，大致2倍的膨胀性需要考虑，但上述成立有条件，Linux也有优化手段

### ②AOF

#### [1]优势

选择appendfsync always方式运行时理论上能够做到数据完整一致，但此时性能又不好。文件内容具备一定可读性，能够用来分析Redis工作情况。

#### [2]劣势

持久化相同的数据，文件体积比RDB大，恢复速度比RDB慢。效率在同步写入时低于RDB，不同步写入时与RDB相同。

### ③RDB和AOF并存

Redis重启的时候会优先载入AOF文件来恢复原始的数据，因为在通常情况下AOF文件保存的数据集要比RDB文件保存的数据集要完整

RDB的数据不实时，同时使用两者时服务器重启也只会找AOF文件。那要不要只使用AOF呢？作者建议不要，因为RDB更适合用于备份数据库(AOF在不断变化不好备份)、快速重启，而且不会有AOF可能潜在的bug，留着作为一个万一的手段。

### ④使用建议

如果Redis仅仅作为缓存可以不使用任何持久化方式。

其他应用方式综合考虑性能和完整性、一致性要求。

RDB文件只用作后备用途，建议只在Slave上持久化RDB文件，而且只要15分钟备份一次就够了，只保留save 900 1这条规则。如果Enalbe AOF，好处是在最恶劣情况下也只会丢失不超过两秒数据，启动脚本较简单只load自己的AOF文件就可以了。代价一是带来了持续的IO，二是AOF rewrite的最后将rewrite过程中产生的新数据写到新文件造成的阻塞几乎是不可避免的。只要硬盘许可，应该尽量减少AOF rewrite的频率，AOF重写的基础大小默认值64M太小了，可以设到5G以上。默认超过原大小100%大小时重写可以改到适当的数值。如果不开启AOF，仅靠Master-Slave Replication 实现高可用性能也不错。能省掉一大笔IO也减少了rewrite时带来的系统波动。代价是如果Master/Slave同时倒掉，会丢失十几分钟的数据，启动脚本也要比较两个Master/Slave中的RDB文件，载入较新的那个。新浪微博就选用了这种架构。

# 九、Redis事务控制

## 1.Redis事务控制的相关命令

| 命令名  | 作用                                                         |
| ------- | ------------------------------------------------------------ |
| MULTI   | 表示开始收集命令，后面所有命令都不是马上执行，而是加入到一个队列中。 |
| EXEC    | 执行MULTI后面命令队列中的所有命令。                          |
| DISCARD | 放弃执行队列中的命令。                                       |
| WATCH   | “观察“、”监控“一个KEY，在当前队列外的其他命令操作这个KEY时，放弃执行自己队列的命令 |
| UNWATCH | 放弃监控一个KEY                                              |

## 2.命令队列执行失败的两种情况

### ①加入队列时失败

``` html
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set age 20
QUEUED
127.0.0.1:6379> incr age
QUEUED
127.0.0.1:6379> incr age www
(error) ERR wrong number of arguments for 'incr' command
127.0.0.1:6379> exec
(error) EXECABORT Transaction discarded because of previous errors.
```

遇到了入队时即可检测到的错误，整个队列都不会执行。

### ②执行队列时失败

``` html
127.0.0.1:6379> multi
OK
127.0.0.1:6379> set age 30
QUEUED
127.0.0.1:6379> incrby age 5
QUEUED
127.0.0.1:6379> incrby age 5
QUEUED
127.0.0.1:6379> incrby age ww
QUEUED
127.0.0.1:6379> incrby age 5
QUEUED
127.0.0.1:6379> EXEC
1) OK
2) (integer) 35
3) (integer) 40
4) (error) ERR value is not an integer or out of range
5) (integer) 45
127.0.0.1:6379> get age
"45"
```

错误在入队时检测不出来，整个队列执行时有错的命令执行失败，但是其他命令并没有回滚。

### ③Redis为什么不支持回滚

官方解释如下：

> 		如果你有使用关系式数据库的经验， 那么 “Redis 在事务失败时不进行回滚，而是继续执行余下的命令”这种做法可能会让你觉得有点奇怪。以下是这种做法的优点：
> 		1.Redis 命令只会因为错误的语法而失败（并且这些问题不能在入队时发现），或是命令用在了错误类型的键上面：这也就是说，从实用性的角度来说，失败的命令是由编程错误造成的，而这些错误应该在开发的过程中被发现，而不应该出现在生产环境中。
> 		2.因为不需要对回滚进行支持，所以 Redis 的内部可以保持简单且快速。
> 		有种观点认为 Redis 处理事务的做法会产生 bug ， 然而需要注意的是， 在通常情况下， 回滚并不能解决编程错误带来的问题。 举个例子， 如果你本来想通过 INCR 命令将键的值加上 1 ， 却不小心加上了 2 ， 又或者对错误类型的键执行了 INCR ， 回滚是没有办法处理这些情况的。

## 3.悲观锁和乐观锁

在使用WATCH命令监控一个KEY后，当前队列中的命令会由于外部命令的执行而放弃，这是乐观锁的体现。

- 悲观锁

  认为当前环境非常容易发生碰撞，所以执行操作前需要把数据锁定，操作完成后释放锁，其他操作才可以继续操作。

- 乐观锁

  认为当前环境不容易发生碰撞，所以执行操作前不锁定数据，万一碰撞真的发生了，那么检查版本号：

  - 如果是基于最新的版本所做的修改：服务器接受，修改成功
  - 如果是基于旧的版本号所做的修改：服务器不接受，修改失败，整个MULTI队列中的操作都被丢弃

# 十、Redis主从复制机制

![p02](./assets/p02.png)

## 1.读写分离的好处：

- 性能优化：主服务器专注于写操作，可以用更适合写入数据的模式工作；同样，从服务器专注于读操作，可以用更适合读取数据的模式工作。
- 强化数据安全，避免单点故障：由于数据同步机制的存在，各个服务器之间数据保持一致，所以其中某个服务器宕机不会导致数据丢失或无法访问。从这个角度说参与主从复制的Redis服务器构成了一个<b><font color="blue">集群</font></b>。

## 2.搭建步骤

### ①思路

Redis集群在运行时使用的是同一个可执行文件，只是对应的配置文件不同。

![p03](./assets/p03.png)

每个配置文件中相同的参数是：

``` html
daemonize yes
dir /usr/local/cluster-redis
```

不同的参数有：

| 配置项名称 | 作用                          | 取值                                                         |
| ---------- | ----------------------------- | ------------------------------------------------------------ |
| port       | Redis服务器启动后监听的端口号 | 6000<br />7000<br />8000                                     |
| dbfilename | RDB文件存储位置               | dump6000.rdb<br />dump7000.rdb<br />dump8000.rdb             |
| logfile    | 日志文件位置                  | /var/logs/redis6000.log<br />/var/logs/redis7000.log<br />/var/logs/redis8000.log |
| pidfile    | pid文件位置                   | /var/run/redis6000.pid<br />/var/run/redis7000.pid<br />/var/run/redis8000.pid |

### ②步骤

- 第一步：创建/usr/local/cluster-redis目录
- 第二步：把原始未经修改的redis.conf复制到/usr/local/cluster-redis目录
- 第三步：把/usr/local/cluster-redis目录下的redis.conf复制为redis6000.conf
- 第四步：按照既定计划修改redis6000.conf中的相关配置项
  - daemonize yes
  - dir
  - port
  - dbfilename
  - logfile
  - pidfile
- 第五步：复制redis6000.conf为redis7000.conf
- 第六步：修改redis7000.conf中的相关配置项
  - port
  - dbfilename
  - logfile
  - pidfile
- 第七步：复制redis6000.conf为redis8000.conf
- 第八步：修改redis8000.conf中的相关配置项
  - port
  - dbfilename
  - logfile
  - pidfile

### ③启动Redis主从复制集群

```html
/usr/local/redis/bin/redis-server /usr/local/cluster-redis/redis6000.conf
/usr/local/redis/bin/redis-server /usr/local/cluster-redis/redis7000.conf
/usr/local/redis/bin/redis-server /usr/local/cluster-redis/redis8000.conf
```

使用redis-cli停止指定服务器的命令格式如下：<br/>
/usr/local/bin/redis-cli -h IP地址 -p 端口号 shutdown

## 3.主从关系

### ①查看主从关系

``` html
127.0.0.1:6000> info replication
# Replication
role:master
connected_slaves:0
```

刚刚启动的集群服务器中每一个节点服务器都认为自己是主服务器。需要建立主从关系。

### ②设定主从关系

在从机上指定主机位置即可

``` html
SLAVEOF 127.0.0.1 6000
```

### ③取消主从关系

在从机上执行命令

``` html
SLAVEOF NO ONE
```

## 4.初步测试

- 测试1：在主机写入数据，在从机查看
- 测试2：在从机写入数据报错。配置文件中的依据是：slave-read-only yes
- 测试3：主机执行SHUTDOWN看从机状态
- 测试4：主机恢复启动，看从机状态
- 测试5：从机SHUTDOWN，此时主机写入数据，从机恢复启动查看状态。重新设定主从关系后看新写入的数据是否同步。

## 5.哨兵模式

### ①作用

通过哨兵服务器监控master/slave实现主从复制集群的自动管理。

![p04](./assets/p04.png)

### ②相关概念

#### [1]主观下线

1台哨兵检测到某节点服务器下线。

#### [2]客观下线

认为某个节点服务器下线的哨兵服务器达到指定数量。这个数量后面在哨兵的启动配置文件中指定。

提示：只有 master 服务器做客观下线的判断， slave 只做主观下线的判定。

#### [3]心跳检查

心跳（heart beat）检查：客户端为了确认服务器端是否正在运行，不断的给服务器端发送数据库包。通过服务器端返回的数据包判断服务器端是否正在运行的工作机制。

### ③配置方式

简单起见我们只配置一台哨兵。我们所需要做的就是创建一个哨兵服务器运行所需要的配置文件。

vim /usr/local/cluster-redis/sentinel.conf

| 格式 | sentinel monitor 为主机命名 主机IP 主机端口号 将主机判定为下线时需要Sentinel同意的数量 |
| ---- | ------------------------------------------------------------ |
| 例子 | sentinel monitor mymaster 127.0.0.1 6000 1                   |

### ④启动哨兵

/usr/local/redis/bin/<span style="color:blue;font-weight:bold;">redis-server</span> /usr/local/cluster-redis/<span style="color:blue;font-weight:bold;">sentinel.conf</span> <span style="color:blue;font-weight:bold;">--sentinel</span>

``` html
+sdown master mymaster 127.0.0.1 6379 【主观下线】
+odown master mymaster 127.0.0.1 6379 #quorum 1/1【客观下线】
……
+vote-for-leader 17818eb9240c8a625d2c8a13ae9d99ae3a70f9d2 1【选举leader】
……
+failover-state-send-slaveof-noone slave 127.0.0.1:6381 127.0.0.1 6381 @ mymaster 127.0.0.1 6379【把一个从机设置为主机】

-------------挂掉的主机又重新启动---------------------
-sdown slave 127.0.0.1:6379 127.0.0.1 6379 @ mymaster 127.0.0.1 6381【离开主观下线状态】
+convert-to-slave slave 127.0.0.1:6379 127.0.0.1 6379 @ mymaster 127.0.0.1 6381【转换为从机】
```

假设两个 slave 服务器参与选举：

- 情况1：
  - slave A：投票给 slave B
  - slave B：投票给 slave A
  - 两个服务器各得一票，平手，不能确定，需要继续投票
- 情况2：
  - slave A：投票给自己
  - slave B：投票给自己
  - 两个服务器各得一票，平手，不能确定，需要继续投票
- 情况3：
  - slave A：投票给自己
  - slave B：投票给 slave A
  - slave A 得 2 票，slave B 没有得票，所以 slave A 当选

# 十一、发布订阅

这个功能不是 Redis 的主要功能，实际开发时发布订阅方面的需求还是要找专门的消息队列产品来完成。

## 1.订阅一个频道

``` html
127.0.0.1:6379> SUBSCRIBE cctv
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "cctv"
3) (integer) 1
```

## 2.在一个频道上发布信息

```html
127.0.0.1:6379> PUBLISH cctv hai
(integer) 1
```

```html
127.0.0.1:6379> SUBSCRIBE cctv
Reading messages... (press Ctrl-C to quit)
1) "subscribe"
2) "cctv"
3) (integer) 1
1) "message"
2) "cctv"
3) "hai"
```

# 十二、Jedis

Jedis 是我们 Java 程序连接 Redis 服务器的客户端。

## 1.一个对比

|          | MySQL                 | Redis     |
| -------- | --------------------- | --------- |
| 连接     | Connection            | Jedis     |
| 连接池   | C3P0、DBCP、Druid等等 | JedisPool |
| 操作完成 | 关闭连接              | 关闭连接  |

## 2.Redis准备

### ①理解Redis配置文件中bind配置项含义

bind后面跟的ip地址是客户端访问Redis时使用的IP地址。规则是：Redis要求客户端访问的地址，必须是 bind 配置项绑定的地址。看下面例子：

| bind值          | 访问方式                       |
| --------------- | ------------------------------ |
| 127.0.0.1       | ./redis-cli -h 127.0.0.1       |
| 192.168.200.100 | ./redis-cli -h 192.168.200.100 |

![p09](./assets/p09.png)

所以，结论是：bind 配置项要绑定可以对外暴露的本机地址。那么 Redis 为什么会有这样的要求？就是因为在实际项目中，Redis 不是给用户直接访问的，而是给 Java 程序访问的。所以 Redis 只要绑定一个内部访问地址，就能够屏蔽外部的访问，所以这个地址绑定机制，能够对 Redis 进行保护。

### ②查看Linux系统本机IP

远程客户端访问Linux服务器时不能使用127.0.0.1，要使用网络上的实际IP。可以用ifconfig命令查看。

### ③将Redis配置文件中的bind配置项设置为本机IP。

``` html
bind [你的实际IP]
bind 192.168.200.100
```

## 3.Jedis

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.9.0</version>
</dependency>
```

``` java
//指定Redis服务器的IP地址和端口号
Jedis jedis = new Jedis("192.168.200.100", 6379);

//执行ping命令
String ping = jedis.ping();

System.out.println(ping);

//关闭连接
jedis.close();

```

## 4.JedisPool

```java
//声明Linux服务器IP地址
String host = "192.168.200.100";

//声明Redis端口号
int port = Protocol.DEFAULT_PORT;

//创建连接池对象
JedisPool jedisPool = new JedisPool(host, port);

//获取Jedis对象连接Redis
Jedis jedis = jedisPool.getResource();

//执行具体操作
String ping = jedis.ping();

System.out.println(ping);

//关闭连接
jedisPool.close();
```

# 十三、Spring Boot Data Redis

## 1.介绍

Spring Data Redis 是 Spring 的一部分，提供了在 Spring 应用中通过简单的配置就可以访问 Redis 服务，对 Redis 底层开发包进行了高度封装。在 Spring 项目中，可以使用Spring Data Redis来简化 Redis 操作。

网址：https://spring.io/projects/spring-data-redis

![image-20210927143741458](./assets/image-20210927143741458.png)

maven坐标：

~~~xml
<dependency>
	<groupId>org.springframework.data</groupId>
	<artifactId>spring-data-redis</artifactId>
	<version>2.4.8</version>
</dependency>
~~~

Spring Boot提供了对应的Starter，maven坐标：

~~~xml
<dependency>
	<groupId>org.springframework.boot</groupId>
	<artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
~~~

Spring Data Redis中提供了一个高度封装的类：**RedisTemplate**，针对 Jedis 客户端中大量api进行了归类封装,将同一类型操作封装为operation接口，具体分类如下：

- ValueOperations：简单K-V操作
- SetOperations：set类型数据操作
- ZSetOperations：zset类型数据操作
- HashOperations：针对hash类型的数据操作
- ListOperations：针对list类型的数据操作

## 2.使用方式

##### ①环境搭建

第一步：创建maven项目springdataredis_demo，配置pom.xml文件

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.4.5</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>top.sharehome</groupId>
    <artifactId>reggie_backend</artifactId>
    <version>1.0-SNAPSHOT</version>
    <properties>
        <java.version>1.8</java.version>
        <maven.compiler.source>8</maven.compiler.source>
        <maven.compiler.target>8</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
    <dependencies>
        <dependency>
            <groupId>cn.hutool</groupId>
            <artifactId>hutool-all</artifactId>
            <version>5.8.11</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <scope>compile</scope>
        </dependency>

        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.2</version>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.20</version>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>fastjson</artifactId>
            <version>1.2.76</version>
        </dependency>

        <dependency>
            <groupId>commons-lang</groupId>
            <artifactId>commons-lang</artifactId>
            <version>2.6</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>

        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-starter</artifactId>
            <version>1.1.23</version>
        </dependency>

        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-core</artifactId>
            <version>4.5.16</version>
        </dependency>

        <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-dysmsapi</artifactId>
            <version>2.1.0</version>
        </dependency>

    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <version>2.4.5</version>
            </plugin>
        </plugins>
    </build>
</project>
~~~

第二步：编写启动类

~~~java
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
~~~

第三步：配置application.yml

~~~yaml
spring:
  application:
    name: application
  #Redis相关配置
  redis:
    host: localhost
    port: 6379
    #password: 123456
    database: 0 #操作的是0号数据库
    jedis:
      #Redis连接池配置
      pool:
        max-active: 8 #最大连接数
        max-wait: 1ms #连接池最大阻塞等待时间
        max-idle: 4 #连接池中的最大空闲连接
        min-idle: 0 #连接池中的最小空闲连接
~~~

解释说明：

> spring.redis.database：指定使用Redis的哪个数据库，Redis服务启动后默认有16个数据库，编号分别是从0到15。
>
> 可以通过修改Redis配置文件来指定数据库的数量。

第四步：提供配置类，主要是实现redis的序列化和反序列化

~~~java
import org.springframework.cache.annotation.CachingConfigurerSupport;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.RedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.serializer.StringRedisSerializer;

/**
 * @Description
 * @Author:AntonyCheng
 * @CreateTime:2023/2/8 00:14
 */
@Configuration
public class RedisConfig extends CachingConfigurerSupport {
    @Bean
    public RedisTemplate<Object, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<Object, Object> redisTemplate = new RedisTemplate<Object, Object>();
        // 没有改变序列化之前的序列化规则是JdkSerializationRedisSerializer();
        // 这里只写键的序列化即可，当程序获取时会将其进行对应的反序列化;
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        return redisTemplate;
    }
}
~~~

解释说明：

> 当前配置类不是必须的，因为 Spring Boot 框架会自动装配 RedisTemplate 对象，但是默认的key序列化器为JdkSerializationRedisSerializer，导致我们存到Redis中后的数据和原始数据有差别

第五步：提供测试类

~~~java
import org.junit.runner.RunWith;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.NoArgsConstructor;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.data.redis.connection.DataType;
import org.springframework.data.redis.core.*;

@SpringBootTest
@RunWith(SpringRunner.class)
public class SpringDataRedisTest {

    @Autowired
    private RedisTemplate redisTemplate;
    
    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    class TestUser {
        private String name;
        private Integer age;
        private String address;
        public final static String NAME = "name";
        public final static String AGE = "age";
        public final static String ADDRESS = "address";
    }
    
}
~~~

##### ②操作String类型数据

~~~java
/**
 * Redis操作String类型数据，redis中String一般用于存储单个数据；
 */
@Test
public void doString() {
    // 首先创建String类型的操作类
    ValueOperations<Object, Object> opsForValue = redisTemplate.opsForValue();

    // 普通增加（修改）
    opsForValue.set("key1", "value1");
    // 定时增加（修改），超时时间为10秒
    opsForValue.set("key2", "value2", 10, TimeUnit.SECONDS);
    // 条件增加（修改），如果key存在就增加（修改）失败
    Boolean ifAbsent = opsForValue.setIfAbsent("key3", "value3");
    System.out.println("ifAbsent = " + ifAbsent);
    // 条件增加（修改），如果key不存在就增加（修改）失败
    Boolean ifPresent = opsForValue.setIfPresent("key4", "value4");
    System.out.println("ifPresent = " + ifPresent);

    // 批量增加
    HashMap<Object, Object> map = new HashMap<Object, Object>();
    for (int i = 0; i < 5; i++) {
        String key = "key" + (i + 5);
        String value = "value" + (i + 5);
        map.put(key, value);
    }
    opsForValue.multiSet(map);

    // 查询
    ArrayList<Object> queryList = new ArrayList<Object>();
    for (int i = 0; i < 9; i++) {
        Object o = opsForValue.get("key" + (i + 1));
        queryList.add(o);
    }
    System.out.println("list = " + queryList.toString());

    // 删除
    ArrayList<Object> deleteList = new ArrayList<Object>();
    for (int i = 0; i < 9; i++) {
        deleteList.add("key" + (i + 1));
    }
    redisTemplate.delete(deleteList);
}
~~~

##### ③操作Hash类型数据

~~~java
/**
 * Redis操作Hash类型数据，redis中Hash一般用于存储对象数据
 */
@Test
public void doHash() {
    // 首先创建Hash类型的操作类
    HashOperations<Object, Object, Object> opsForHash = redisTemplate.opsForHash();

    // 增加（修改）对象（name，age，address单个字段）
    TestUser testUser1 = new TestUser("Antony1", 21, "HRBCU1");
    opsForHash.put("person1", TestUser.NAME, testUser1.getName());
    opsForHash.put("person1", TestUser.AGE, testUser1.getAge());
    opsForHash.put("person1", TestUser.ADDRESS, testUser1.getAddress());
    // 增加（修改）对象（Map对象）
    TestUser testUser2 = new TestUser("Antony2", 22, "HRBCU2");
    HashMap<Object, Object> map = new HashMap<Object, Object>();
    map.put(TestUser.NAME, testUser2.getName());
    map.put(TestUser.AGE, testUser2.getAge());
    map.put(TestUser.ADDRESS, testUser2.getAddress());
    opsForHash.putAll("person2", map);

    // 查询对象（单个字段查询后组成一个对象）
    String name1 = (String) opsForHash.get("person1", TestUser.NAME);
    Integer age1 = (Integer) opsForHash.get("person1", TestUser.AGE);
    String address1 = (String) opsForHash.get("person1", TestUser.ADDRESS);
    TestUser queryPerson1 = new TestUser(name1, age1, address1);
    System.out.println("queryUser1 = " + queryPerson1);

    // 查询对象（entries查询）
    Map<Object, Object> mapEntry = opsForHash.entries("person1");
    TestUser queryPerson2 = new TestUser((String) mapEntry.get(TestUser.NAME), (Integer) mapEntry.get(TestUser.AGE), (String) mapEntry.get(TestUser.ADDRESS));
    System.out.println("queryPerson2 = " + queryPerson2);

    // 查询对象（keys,values查询）
    Set<Object> keys = opsForHash.keys("person1");
    List<Object> keyList = new ArrayList<>(keys);
    List<Object> valueList = opsForHash.values("person1");
    HashMap<Object, Object> hashMap = new HashMap<Object, Object>();
    for (int i = 0; i < keys.size(); i++) {
        hashMap.put(keyList.get(i), valueList.get(i));
    }
    TestUser queryPerson3 = new TestUser((String) hashMap.get(TestUser.NAME), (Integer) hashMap.get(TestUser.AGE), (String) hashMap.get(TestUser.ADDRESS));
    System.out.println("queryPerson3 = " + queryPerson3);

    // 删除对象
    opsForHash.delete("person1", TestUser.NAME, TestUser.AGE, TestUser.ADDRESS);
    opsForHash.delete("person2", TestUser.NAME, TestUser.AGE, TestUser.ADDRESS);
}
~~~

##### ④操作List有序可重复集合类型数据

~~~java
/**
 * Redis操作List有序可重复集合类型数据，redis中List一般用于存储多个数据，该数据存储时保持相同顺序且可重复
 * List可以看成一个队列，所以redis中的List数据类型也可以用来做消息队列使用
 */
@Test
public void doList() {
    // 首先创建List类型的操作类
    ListOperations<Object, Object> opsForList = redisTemplate.opsForList();

    // 向左（头部）添加一个数据（向右即为rightPush）
    opsForList.leftPush("list", 1);
    // 向左（头部）添加多个数据（向右即为rightPushAll）
    opsForList.leftPushAll("list", 5, 2, 4, 3);

    // 查询数据（需要查询范围）
    List<Object> list1 = opsForList.range("list", 0, -1);
    System.out.println("list = " + list1);

    // 向右（尾部）弹出一个数据（向左即为leftPop）
    // 利用循环弹出全部数据，弹出完毕后该List即被删除
    for (int i = 0; i < list1.size(); i++) {
        Object exit = opsForList.rightPop("list");
        System.out.println("第" + (i + 1) + "次弹出结果为：" + exit);
    }
}
~~~

##### ⑤操作Set无序不重复集合类型数据

~~~java
/**
 * Redis操作Set无序不重复集合类型数据，redis中Set一般用于存储多个数据，该数据存储时不会保持相同顺序且不可重复
 */
@Test
public void doSet() {
    // 首先创建Set类型的操作类
    SetOperations<Object, Object> opsForSet = redisTemplate.opsForSet();

    // 添加（多个）数据
    opsForSet.add("set", 1, 1, 3, 3, 2, 2);

    // 查询数据
    Set<Object> members1 = opsForSet.members("set");
    System.out.println("members = " + members1);

    // 删除数据
    opsForSet.remove("set", 1, 2, 3);
}
~~~

##### ⑥操作ZSet权重排序不重复集合类型数据

~~~java
/**
 * Redis操作ZSet权重排序不重复集合类型数据，redis中Set一般用于存储多个数据，该数据存储时会按照给定权重排序且可不重复
 */
@Test
public void doZSet() {
    // 首先创建ZSet类型的操作类
    ZSetOperations<Object, Object> opsForZSet = redisTemplate.opsForZSet();

    // 添加数据和对应权重，当重复的数据时，权重会被覆盖，redis会按照权重由小到大排序
    opsForZSet.add("zset", 1, 10.0);
    opsForZSet.add("zset", 2, 11.0);
    opsForZSet.add("zset", 3, 12.0);
    opsForZSet.add("zset", 1, 13.0);
    opsForZSet.add("zset", 100, 14.0);

    // 查询数据
    Set<Object> zset1 = opsForZSet.range("zset", 0, -1);
    System.out.println("zset1 = " + zset1);

    // 改变权重
    opsForZSet.incrementScore("zset", 2, 20.0);
    Set<Object> zset2 = opsForZSet.range("zset", 0, -1);
    System.out.println("zset2 = " + zset2);

    // 删除数据
    opsForZSet.remove("zset", 100, 1, 2, 3);
}
~~~

##### ⑦Redis中的通用操作

~~~java
/**
 * Redis中的通用操作
 */
@Test
public void testGeneral() {
    // 获取redis中所有的key
    Set<Object> keys = redisTemplate.keys("*");
    System.out.println("keys = " + keys);

    // 判断某个key是否存在
    Boolean hasKey = redisTemplate.hasKey("backup1");
    System.out.println("hasKey = " + hasKey);

    // 删除指定key
    Boolean delete = redisTemplate.delete("backup1");
    System.out.println("delete = " + delete);

    // 获取指定key对应的value的数据类型
    DataType studentType = redisTemplate.type("backup2");
    System.out.println("studentType = " + studentType);
}
~~~

# 十四、总结

- 理论：理解
  - 新时代互联网项目对后端开发提出的新挑战
  - Redis 持久化机制
  - Redis 事务控制
  - Redis 主从复制、哨兵模式
  - Redis 发布订阅（了解）
- 操作：练习
  - Redis 程序安装
  - Redis 数据结构
    - 基本数据结构：string、list、hash、set、zset
    - 衍生数据结构：Geo、HyperLoglogs、bitmap
  - 主从复制、哨兵模式
  - Jedis

