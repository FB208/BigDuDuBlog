---
{"dg-publish":true,"permalink":"/03-网络技术/查看IP是否被墙/","dgPassFrontmatter":true,"updated":"2023-11-09T17:13:53.000+08:00"}
---



境外站地址
https://www.yougetsignal.com/tools/open-ports/

国内地址
https://tool.chinaz.com/port

# 使用方法

分别在两个网址进行测试，填入相同的IP，端口号(推荐80)

# 结论

| 国内站 | 国外站 | 结论         | 备注 |
|:------ |:------ |:------------ |:---- |
| 开启   | OPEN   | 正常         |      |
| 开启   | CLOSE  | 境外不能访问 |      |
| 关闭   | OPEN   | 被墙         |      |
| 关闭   | CLOSE  | 端口未放开   |在被测端口部署个小网站，用localhost访问看本机端口是否正常，正常的话检查防火墙|



