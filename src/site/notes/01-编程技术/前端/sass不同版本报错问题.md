---
{"dg-publish":true,"permalink":"/01-编程技术/前端/sass不同版本报错问题/","dgPassFrontmatter":true,"created":"2023-10-26T22:43:44.230+08:00","updated":"2024-01-11T08:37:40.000+08:00"}
---

![image.png](/img/user/assets/1654092309353-90ef6ef2-60ae-4701-80cd-319e4651916c.png)

```xml
由于sass-loader的版本不同，这里可能会报错，不同的版本对应的关键字不一样：
sass-loader v8-中，关键字为 “ data ”
sass-loader v8中，关键字为 “ prependData ”
sass-loader v10+中，关键字为 “ additionalData ”
（如果是在webpack.config.js中配置scss）,可根据https://blog.csdn.net/weixin_43846248/article/details/89056997进行配置
```
