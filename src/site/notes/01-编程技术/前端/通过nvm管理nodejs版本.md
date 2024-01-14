---
{"dg-publish":true,"permalink":"/01-编程技术/前端/通过nvm管理nodejs版本/","dgPassFrontmatter":true,"created":"2023-10-26T22:42:22.871+08:00","updated":"2024-01-11T08:37:37.000+08:00"}
---


下载：[coreybutler/nvm-windows: A node.js version management utility for Windows. Ironically written in Go. (github.com)](https://github.com/coreybutler/nvm-windows)

备用地址： http://kod.bigdudu.cn:6688/#s/9kQznriA


# 常用命令

``` shell
# 查看所有可用版本
nvm ls available

# 安装指定版本
nvm install 18.17.1

# 查看已安装版本
nvm list

# 应用版本
nvm use 18.17.1

# 查看node版本
node -v

# 删除指定版本
nvm uninstall 18.17.1
```