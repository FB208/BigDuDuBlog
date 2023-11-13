---
{"dg-publish":true,"permalink":"/01-编程技术/前端/NodeJs及NPM常用指令/","dgPassFrontmatter":true,"created":"2023-10-27T09:00:35.315+08:00","updated":"2023-11-13T19:46:43.066+08:00"}
---


# nodejs版本
```shell
# 查看版本
node -v
# 升级
windows直接下载最新版安装即可
```

# npm版本
```shell
# 查看版本
npm -v
# 升级
npm install -g npm
```

# 查看包支持版本
npm view less-loader versions


# 修改至阿里仓库地址
```
npm config set registry https://registry.npm.taobao.org
```


# npm install 旧项目版本问题
```java
npm install --legacy-peer-deps
```

在NPM v7中，现在默认安装peerDependencies
在很多情况下，这会导致版本冲突，从而中断安装过程。
--legacy-peer-deps标志是在v7中引入的，目的是绕过peerDependency自动安装；
它告诉 NPM 忽略项目中引入的各个modules之间的相同modules但不同版本的问题并继续安装，保证各个引入的依赖之间对自身所使用的不同版本modules共存。
