---
{"dg-publish":true,"permalink":"/01-编程技术/RocketMQ-Windows部署RocketMq/","dgPassFrontmatter":true,"created":"2023-10-26T22:43:43.958+08:00","updated":"2023-11-09T17:11:34.000+08:00"}
---

> 本文由 [简悦 SimpRead](http://ksria.com/simpread/) 转码， 原文地址 [blog.csdn.net](https://blog.csdn.net/qq_45872465/article/details/122961677?spm=1001.2014.3001.5502)



## 一、[RocketMQ](https://so.csdn.net/so/search?q=RocketMQ&spm=1001.2101.3001.7020) 介绍

1、开发指南：[Gitee 中文学习地址](https://gitee.com/apache/rocketmq/tree/master/docs/cn#/apache/rocketmq/blob/master/docs/cn/best_practice.md) ([https://www.processon.com/view/link/620c69d95653bb4ec5bb75cd#map](https://www.processon.com/view/link/620c69d95653bb4ec5bb75cd#map))


## 二、RocketMQ 下载

[二进制版本 4.9.2 官方下载](https://www.apache.org/dyn/closer.cgi?path=rocketmq/4.9.2/rocketmq-all-4.9.2-bin-release.zip)


## 三、安装部署过程 (带！为非必要操作)

**准备工作：已配置 Java 环境**

```
classpath	.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;(新建)
JAVA_HOME	E:\runtools\java8(新建)
Path		%JAVA_HOME%\jre\bin;%JAVA_HOME%\bin;(添加)
```

![](https://img-blog.csdnimg.cn/f8e215d0421b4bf180eed95054c5365f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_11,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=PBCrt&originHeight=531&originWidth=455&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

备注：java 环境 JAVA_HOME 路径中不要携带空格，RocketMQ 启动程序引用了很多 CLASS_PATH、JAVA_HOME，避免路径中空格带来不必要的麻烦，一次性将 java 环境更换目录<br />JAVA_HOME 更换目录作简述：安装 Java(64 位，且版本号和原有不同的版本) 环境至不带空格的文件夹，随即修改 JAVA_HOME 即可。

**1、RocketMQ 解压安装包目录**  
![](https://img-blog.csdnimg.cn/5a97042affa84fd09561864b9827d120.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_14,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=d9Byr&originHeight=294&originWidth=576&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
**2、配置 RocketMQ 全局环境变量**

```
变量名：ROCKETMQ_HOME
变量值：E:\runtools\rocketmq\rocketmq-4.9.2
```

![](https://img-blog.csdnimg.cn/5ca4908c571944abb1fb974ed88f9a99.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_20,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=U8kdY&originHeight=671&originWidth=1380&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

**3、内存分配设置 (日后可能占用较高内存，根据服务器后续自行调整配置)！**

**3-1 编辑 rocketmq-4.9.2\bin\runserver.cmd**

```
rem set "JAVA_OPT=%JAVA_OPT% -server -Xms2g -Xmx2g -Xmn1g -XX:MetaspaceSize=128m -XX:MaxMetaspaceSize=320m"
set "JAVA_OPT=%JAVA_OPT% -server -Xms256m -Xmx512m"
```

[minio 文件服务器 Windows 版 exe  0 星 超过 10% 的资源 49.05MB  下载](https://download.csdn.net/download/kaka_qwerty/12832725)![](https://csdnimg.cn/release/blogv2/dist/components/img/star.png#crop=0&crop=0&crop=1&crop=1&id=XDoLY&originHeight=26&originWidth=26&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)![](https://csdnimg.cn/release/blogv2/dist/components/img/arrowDownWhite.png#crop=0&crop=0&crop=1&crop=1&id=fijTi&originHeight=32&originWidth=32&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

![](https://img-blog.csdnimg.cn/3684935a3c3943cb85106db2f92d0f87.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_20,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=Lj3Dk&originHeight=619&originWidth=1642&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

**3-2 编辑 rocketmq-4.9.2\bin\runbroker.cmd**  

```
rem set "JAVA_OPT=%JAVA_OPT% -server -Xms2g -Xmx2g"
set "JAVA_OPT=%JAVA_OPT% -server -Xms256m -Xmx512m"
```

![](https://img-blog.csdnimg.cn/5e2fd08aff9a45a7a9bb96c1dd13b0b2.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_20,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=ksbu4&originHeight=491&originWidth=1282&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
**4、配置日志目录 (默认日志在 C 盘，且日志文件较大)！**

修改日志配置文件  
![](https://img-blog.csdnimg.cn/264bb60589154ed898a00a6522ebd9c4.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_18,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=Ih0cD&originHeight=419&originWidth=742&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
将两个配置文件添加如下配置，且将原 user.home 替换为 LOG_BASE
```
<property  />
```

![](https://img-blog.csdnimg.cn/fc5afdf8442d4a0687f3d250374438fc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_20,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=NjB3P&originHeight=613&originWidth=1219&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

**5、启动 mqnamesrv、mqbroker.cmd**  

.cmd 命令内容：先启动 namesrv、然后启动 broker  
```
cd E:\runtools\rocketmq\rocketmq-4.9.2\bin
E:
start mqnamesrv.cmd
start mqbroker.cmd -n 0.0.0.0:9876 -c E:\runtools\rocketmq\rocketmq-4.9.2\conf\broker.conf
```

**6、启动成功**
如图所示即启动成功  
![](https://img-blog.csdnimg.cn/91c9f0cae9d44dc587f9f4b8ab511f5f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_20,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=k2kLg&originHeight=453&originWidth=1376&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)


## 四、RocketMQ 图形化管理控制台

1、**源文件下载**  
[Github 下载地址](https://github.com/apache/rocketmq-externals)  
![](https://img-blog.csdnimg.cn/cf6a6bbf388e48e1a99665827a075cde.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_14,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=S8KJy&originHeight=650&originWidth=603&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

2、**更改配置**  
![](https://img-blog.csdnimg.cn/1c4e8ac2525c4d7eac9f4492b802589f.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_20,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=eC7TV&originHeight=692&originWidth=1013&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
3、**这里我采用打包部署服务器的方式**  
![](https://img-blog.csdnimg.cn/00f24a4099044859b038757265fe7249.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_8,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=wExoI&originHeight=361&originWidth=339&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)

4、**控制端页面访问**  
![](https://img-blog.csdnimg.cn/2a543969b40d48468d706d35fa2f28ca.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5a2m5LiN5Lya55qE5bCP5YWt5a2Q,size_20,color_FFFFFF,t_70,g_se,x_16#crop=0&crop=0&crop=1&crop=1&id=lbRyR&originHeight=665&originWidth=1692&originalType=binary&ratio=1&rotation=0&showTitle=false&status=done&style=none&title=)
