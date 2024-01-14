---
{"dg-publish":true,"permalink":"/01-编程技术/JAVA/自制免安装JDK/","dgPassFrontmatter":true,"created":"2023-10-26T22:42:22.819+08:00","updated":"2024-01-11T08:37:48.000+08:00"}
---




# 前言

oracle官网提供的windows平台jdk安装包是exe格式，需要安装，个人觉得挺麻烦的，要是像linux平台的包那样，解压到目录配置个环境变量就可以用就挺方便的。本文旨在记录从jdk安装包exe文件抽取文件并自行打包为免安装版的过程。

# 制作过程

首先，将exe文件使用压缩软件解压，目录结构如下(使用版本jdk-8u201-windows-x64.exe)   
![在这里插入图片描述](/img/user/assets/1692603870-c3f2b886f5a80518faf75feba905079f.png)

我们将 `.rsrc\1033\JAVA_CAB9` 下的名为110的文件解压，提取到当前位置，多出一个src.zip文件，这是源码包 
![在这里插入图片描述](/img/user/assets/1692603870-1916acbc32fe6cda9df68e06fda2052b.png) 我们将 `.rsrc\1033\JAVA_CAB10` 下111文件解压，提取到当前位置，多出一个tool.zip 
![在这里插入图片描述](/img/user/assets/1692603870-3048dac82ecb644a8ac90336e01055cf.png) tool.zip内的内容如下 
![在这里插入图片描述](/img/user/assets/1692603870-e73427abc1b39035011576e9d2b155fb.png)

然后，我们创建一个jdk文件夹，保存免安装版jdk文件。将上述的tool.zip解压到jdk目录，并将src.zip直接拷贝到jdk目录。完成这些操作后，jdk目录的结构如下 
![在这里插入图片描述](/img/user/assets/1692603870-dd3447aad6a4d8ac39d330a43c485059.png) 最后，将jdk目录中一些 .pack 文件 转为 .jar 文件。进入jdk目录，cmd输入命令

for /r %x in (*.pack) do .\bin\unpack200 -r "%x" "%~dx%~px%~nx.jar"

![在这里插入图片描述](/img/user/assets/1692603870-8b201de6644d730bd5b34559c1c79f5c.png) 完成！最后这个jdk文件夹可以作为绿色版jdk使用，随拷随用。建议打为压缩包，想要装哪解压到哪。

