---
{"dg-publish":true,"title":"1-debian安装docker","categories":[],"tags":[],"halo":{"site":"http://121.41.99.125:8090","name":"c629ca02-b9f3-4175-a6a4-3f57d5c6fefb","publish":false},"permalink":"/04-Docker技术/1-debian安装docker/","dgPassFrontmatter":true,"created":"2023-10-27T09:00:33.352+08:00","updated":"2024-01-22T09:37:34.000+08:00"}
---

#debian装机系列 

按照官网的安装教程会有几个小坑，踩坑后梳理一下安装步骤

适用于Debian10、Debain11
Docker 23.0.1

# 安装前检查和准备

***确保iptables可用***
Docker安装依赖于iptables，首先确保开发板中的iptables命令正常可用。

在使用过程中可能会碰到iptables报错，由于docker是用iptables初始化NAT网络，而Debian buster使用 nftables 而不是 iptables，导致dockerd不能正常完成NAT初始化，出错退出。

处理方法是调用update-alternatives强制Debian用iptables而不是nftables。
```
# for ipv4
update-alternatives --set iptables /usr/sbin/iptables-legacy
# for ipv6
update-alternatives --set ip6tables /usr/sbin/ip6tables-legacy
```

***卸载旧版docker***
```
apt-get remove docker docker-ce docker-engine docker.io containerd runc
```

# 开始安装

***安装依赖环境***
``` shell
# 此项必须
apt-get update

 apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
```

***添加 Docker 的官方 GPG 密钥***
在docker下载的过程中，需要使用到GPG密钥，使用curl命令来添加GPG密钥。
```
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

**注意：**
在这个过程中可能会碰到curl报错，需要下载curl证书，
从这个地址：https://curl.se/docs/caextract.html 下载cacert.pem，再将这个文件添加至环境变量（注意路径），就可以执行上述命令了。

```
wget https://curl.se/ca/cacert.pem
```

***设置官方源***
```
 echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian \
  $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null
```

***安装docker-ce***
```
apt-get update
```

```
apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

# 检查是否安装成功
```
docker version
```
![](/img/user/assets/Pasted image 20230220094907.png)

# 单独安装docker-compose
注意下面的版本号自己去github查

``` shell
# 下载 Docker Compose 的当前稳定版本  
curl -L "https://github.com/docker/compose/releases/download/2.18.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose  
# 配置权限
chmod +x /usr/local/bin/docker-compose  
# 创建软链  
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose  
# 测试是否安装成功  
docker-compose --version
```

访问不了github也可以自己到 https://github.com/docker/compose/ 下载对应版本拷贝到服务器上/usr/local/bin，重命名成docker-compose
然后 
``` shell
# 配置权限
chmod +x /usr/local/bin/docker-compose  
# 创建软链  
ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose  
# 测试是否安装成功  
docker-compose --version
```

# 改成阿里源

![Docker改成阿里源](10-读书视频笔记/狂神docker/Docker改成阿里源.md)
