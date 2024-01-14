---
{"dg-publish":true,"permalink":"/01-编程技术/DotNET/DotNet操作Redis/","dgPassFrontmatter":true,"created":"2023-10-26T22:42:22.855+08:00","updated":"2023-11-09T17:12:45.000+08:00"}
---


# 为什么选择CSRedisCore
 
ServiceStack.Redis 是商业版，免费版有限制；
StackExchange.Redis 是免费版，但是内核在 .NETCore 运行有问题经常 Timeout，暂无法解决；
CSRedis于2016年开始支持.NETCore一直迭代至今，实现了低门槛、高性能，和分区高级玩法的.NETCore redis-cli SDK；
在v3.0版本更新中，CSRedis中的所有方法名称进行了调整，使其和redis-cli保持一致，如果你熟悉redis-cli的命令的话，CSRedis可以直接上手，这样学习成本就降低很多。

## 初始化

```html
using CSRedis;

namespace RedisCommon
{
    public class RedisInit
    {
        public static void RedisInitialization()
        {
            var csRedis = new CSRedisClient($"60.30.198.42:11379,defaultDatabase=0,prefix=fas2dmc");
            RedisHelper.Initialization(csRedis);
        }
    }
}
```

```html
static void Main(string[] args)
{
    //初始化Redis
    RedisInit.RedisInitialization();
}
```


## 链接字符串详解

 127.0.0.1:6379,password=YourPassword,defaultDatabase=0,prefix=hr\_

| Parameter  	|  Default 	| Explain  	| 说明  	|
|---	|---	|---	|---	|
|  password 	| 　 	|  Redis server password 	|  Redis服务器密码 	|
|  defaultDatabase 	|  0 	|  Redis server database 	|  Redis服务器数据库 	|
|  asyncPipeline 	|  false 	|  The asynchronous method automatically uses   pipeline, and the 10W concurrent time is 450ms (welcome to feedback) 	|  异步方式自动使用管道，10W并发时间450ms（欢迎反馈） 	|
|  poolsize 	|  50 	|  Connection pool size 	|  连接池大小 	|
|  idleTimeout 	|  20000 	|  idle time of elements in the connection   pool(MS),suitable for connecting to remote redis server  	|  连接池中元素的空闲时间（MS），适合连接到远程redis服务器 	|
|  connectTimeout 	|  5000 	|  Connection timeout(MS)  	|  连接超时（毫秒） 	|
|  syncTimeout 	|  10000 	|  Send / receive timeout(MS)  	|  发送/接收超时（毫秒） 	|
|  preheat 	|  5 	|  Preheat connections, receive values such as   preheat = 5 preheat 5 connections 	|  预热连接，接收值，例如Preheat=5   Preheat 5 connections 	|
|  autoDispose 	|  true 	|  Follow system exit event to release   automatically  	|  跟随系统退出事件自动释放 	|
|  ssl 	|  false 	|  Enable encrypted transmission 	|  启用加密传输 	|
|  testcluster 	|  true 	|  是否尝试集群模式，阿里云、腾讯云集群需要设置此选项为false 	| 　 	|
|  tryit 	|  0 	|  Execution error, retry  attempts 	|  执行错误，重试次数 	|
|  name 	| 　 	|  Connection name, use client list command to   view 	|  连接名称，使用client   list命令查看 	|
|  prefix 	| 　 	|  key前缀，所有方法都会附带此前缀，csredis.Set(prefix +   "key", 111); 	| 　 	|



## 常用代码
### 通用指令

``` C#
//查找所有分区节点中符合给定模式(pattern)的 key
string[] keyAll = RedisHelper.Keys("*");

//以秒为单位，返回给定 key 的剩余生存时间
long ttl1 = RedisHelper.Ttl("keyString1");

//用于在 key 存在时删除 key
long del1 = RedisHelper.Del("keyString1");

//检查给定 key 是否存在
bool isExists1 = RedisHelper.Exists("keyString1");

//为给定 key 设置过期时间
bool isExpire1 = RedisHelper.Expire("keyString1", 100);

//为给定 key 设置过期时间
RedisHelper.ExpireAt("keyString1", new DateTime(2021, 6, 11, 16, 0, 0));
```

### string（字符串）

*   简单操作

``` C#
// 设置指定 key 值，默认不过期
bool set_string1 = RedisHelper.Set("keyString_String1", "测试值1");

// 设置指定 key 值，并设置过期时间（单位：秒）
bool set_string2 = RedisHelper.Set("keyString_String2", "测试值2", 1);

// 获取指定 key 的值，不存在的 key，值返回null
string get_string1 = RedisHelper.Get("keyString_String1");

// 获取指定 key 的值，不存在的 key，或者指定的 key 不是int型，则返回int类型的默认值0
int get_int1 = RedisHelper.Get<int>("keyString_String1");
```

*   对整数类型进行自增，自减操作

```C#
bool set_int1 = RedisHelper.Set("keyString_Num1", "23");

// 将 key 所储存的值加上指定的增量值（increment）
long incrBy1 = RedisHelper.IncrBy("keyString_Num1", 2);// #25

// 将 key 所储存的值加上指定的增量值（increment），负数就是减量值
long incrBy2 = RedisHelper.IncrBy("keyString_Num1", -1);// #24
```

*   在指定 key 的 value 末尾追加字符串

```C#
bool set_append1 = RedisHelper.Set("keyString_Append1", "qaz", 30);

// 将指定的 value 追加到该 key 原来值（value）的末尾
long append1 = RedisHelper.Append("keyString_Append1", "wsx");// #6 结果：key 中字符串的长度
```

###    hash（哈希）

*   HSet、HGet、HDel方法 \[只能处理一个键值对\]

```C#
// 将哈希表 key 中的字段 field 的值设为 value
bool set_hash_user1_uname = RedisHelper.HSet("User:10001", "uname", "gongyg"); // 冒号的作用相当于创建一个文件夹
bool set_hash_user1_upwd = RedisHelper.HSet("User:10001", "upassword", "123456");
bool set_hash_user1_uid = RedisHelper.HSet("User:10001", "uid", "12");

// 获取存储在哈希表中指定字段的值
string uName = RedisHelper.HGet("User:10001", "uname");

// 获取存储在哈希表中指定字段的值，并指定类型
int uId = RedisHelper.HGet<int>("User:10001", "uid");

// 删除一个或多个哈希表字段，不能删除key
long hDel1 = RedisHelper.HDel("User:10001", "uname");
```

*   HGetAll、HKeys、HVals

```C#
// 获取在哈希表中指定 key 的所有字段和值
Dictionary<string, string> user10001 = RedisHelper.HGetAll("User:10001");
foreach (var item in user10001)
{
    string key = item.Key;
    string value = item.Value;
}

// 获取所有哈希表中的字段 [虽然使用HGetAll可以取出所有的value，但是有时候散列包含的值可能非常大，容易造成服务器的堵塞，为了避免这种情况，我们可以使用HKeys取到散列的所有键(HVals可以取出所有值)，然后再使用HGet方法一个一个地取出键对应的值。]
string[] fields = RedisHelper.HKeys("User:10001");
foreach (string item in fields)
{
    string val = RedisHelper.HGet("User:10001", item);
}

// 获取哈希表中所有的值
string[] vals = RedisHelper.HVals("User:10001");
```

*   HMSet、HMGet \[HGet和HSet方法执行一次只能处理一个键值对，而HMGet和HMSet是他们的多参数版本，一次可以处理多个键值对。\]

```C#
//var keyValues = dic.Select(a => new [] { a.Key, a.Value.ToString() }).SelectMany(a => a).ToArray();
string[] user2 = new string[] { "uname", "gmd", "upwd", "123" };
// 同时将多个field-value（域-值）对设置到哈希表 key 中
bool set_hash_user2 = RedisHelper.HMSet("User:10002", user2);

string[] user_get2 = new string[] { "uname", "upwd", "sj" };
// 获取存储在哈希表中多个字段的值
string[] user_val2 = RedisHelper.HMGet("User:10002", user_get2); // #gmd,123,
```

*   对散列中的值进行自增、自减操作

```C#
bool set_hash_user1_usex = RedisHelper.HSet("User:10003", "uage", "23");

// 为哈希表 key 中的指定字段和整数值加上增量（increment）自增（正数），自减（负数）
long hIncrBy = RedisHelper.HIncrBy("User:10001", "uage", 2);
```

###    list（列表）

```html
// 将一个或多个值插入到列表头部
string[] lpush1 = new string[] { "003", "004" };
long len1 = RedisHelper.LPush("list", "000");
long len2 = RedisHelper.LPush("list", "001", "002");
long len3 = RedisHelper.LPush("list", lpush1);

// 在列表中添加一个或多个值 [列表尾部]
long len4 = RedisHelper.RPush("list", "010");

// 移除并获取列表的第一个元素
string val1 = RedisHelper.LPop("list");

// 移除并获取列表的最后一个元素
string val2 = RedisHelper.RPop("list");

// 获取列表指定范围内的元素[key, start, stop]
string[] lrang1 = RedisHelper.LRange("list", 0, 2); // #左侧开始，获取前3个元素
string[] lrang2 = RedisHelper.LRange("list", 0, -1); // #左侧开始，获取全部元素

// 将 list 最后一个元素弹出并压入 list_another 的头部 [只有一个元素的改变，源列表会少一个元素，目标列表多出一个元素]
RedisHelper.RPopLPush("list", "list_another");
RedisHelper.Expire("list_another", 30);
```

###    set（无序集合）

*   对集合中的成员进行操作

```html
// 向集合添加一个或多个成员 [返回添加成功个数]
long sadd1 = RedisHelper.SAdd("my_set", "qaz");
long sadd2 = RedisHelper.SAdd("my_set", "tgb", "yhn");
string[] set1 = new string[] { "wsx", "edc" , "rfv" };
long sadd3 = RedisHelper.SAdd("my_set", set1);

// 判断 member 元素是否是集合 key 的成员
bool isMember = RedisHelper.SIsMember("my_set", "qaz");

// 返回集合中的所有成员
string[] members = RedisHelper.SMembers("my_set");

// 返回集合中的一个随机成员
string member1 = RedisHelper.SRandMember("my_set");

// 移除集合中一个或多个成员
long sRem = RedisHelper.SRem("my_set", "qaz");

// 移除并返回集合中一个随机成员
string member2 = RedisHelper.SPop("my_set");
```

*   对两个集合进行交、并、差操作

```html
RedisHelper.SAdd("set-a", "item1", "item2", "item3", "item4", "item5");
RedisHelper.SAdd("set-b", "item2", "item5", "item6", "item7");

// 差集
RedisHelper.SDiff("set-a", "set-b"); // "item1", "item3","item4"

// 交集
RedisHelper.SInter("set-a", "set-b"); // "item2","item5"

// 并集
RedisHelper.SUnion("set-a", "set-b"); // "item1","item2","item3","item4","item5","item6","item7"

//#另外还可以用SDiffStore,SInterStore,SUnionStore将操作后的结果存储在新的集合中。
```

###    zset（sorted set：有序集合）

*   有序集合可以看作是可排序的散列，不过有序集合的val成为score分值，集合内的元素就是基于score进行排序的，score以双精度浮点数的格式存储。

```html
// 向有序集合添加一个或多个成员，或者更新已存在成员的分数
RedisHelper.ZAdd("sorted_set", (1, "beijing"));
RedisHelper.ZAdd("sorted_set", (2, "shanghai"), (3, "shenzhen"));
(decimal, object)[] set1 = new (decimal, object)[] { (4, "guangzhou"), (5, "tianjing"), (6, "chengdu") };
RedisHelper.ZAdd("sorted_set", set1);

// 有序集合中对指定成员的分数加上增量 increment
decimal incr = RedisHelper.ZIncrBy("sorted_set", "beijing", -2);

// 通过索引区间返回有序集合成指定区域内的成员，分数从低到高 [key, start, stop]
string[] zRange1 = RedisHelper.ZRange("sorted_set", 0, 2);
string[] zRange2 = RedisHelper.ZRange("sorted_set", 0, -1); // #stop=-1返回全部

// 返回有序集合中指定区域内的成员，通过索引，分数从高到底 [key, start, stop]
string[] zRevRange1 = RedisHelper.ZRevRange("sorted_set", 0 , 2);
string[] zRevRange2 = RedisHelper.ZRevRange("sorted_set", 0, -1); // #stop=-1返回全部

// 移除有序集合中一个或多个成员
RedisHelper.ZRem("sorted_set", "shenzhen");

// 获取有序集合的成员数量
long number = RedisHelper.ZCard("sorted_set");

// 通过分数返回有序集合指定区间内的成员
string[] ZRangByScore1 = RedisHelper.ZRangeByScore("sorted_set", 2, 4);

// 通过索引区间返回有序集合成指定区间内的成员和分数
(string member, decimal score)[] sets = RedisHelper.ZRangeWithScores("Quiz", 0, -1);
```

###    Geo（经纬度）

```html
//1. 添加地点经纬度 [存储到 sorted set 中]
RedisHelper.GeoAdd("myLocation", Convert.ToDecimal(116.20), Convert.ToDecimal(39.56), "北京");
RedisHelper.GeoAdd("myLocation", Convert.ToDecimal(120.51), Convert.ToDecimal(30.40), "上海");

//2. 求两点之间的距离
var d1 = RedisHelper.GeoDist("myLocation", "北京", "上海", GeoUnit.km);
```

###    事务

```html
// 开启事务
var pipe = RedisHelper.StartPipe();

//中间对redis进行操作
pipe.Set("pipe1", "wsx");

// 提交
pipe.EndPipe();
```
