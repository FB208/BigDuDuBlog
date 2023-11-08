---
{"dg-publish":true,"dg-home":true,"permalink":"/06-Obsidian技术/Obsidian发布博客/","tags":["gardenEntry"],"dgPassFrontmatter":true}
---

#博客 #blob #netlify #github

# 原理
obsidian通过Digital Garden插件，将md推送到github，再由netlify代理github。
阿里云bigdudu.cn通过cname解析到netlify的域名，并会自动配置ssl证书

# 发布博客方法
obsidian添加文档属性
``` yaml
//是否发布
dg-publish: true
//发布为首页
dg-home: true
```

![assets/Pasted image 20231108225147.png](/img/user/assets/Pasted%20image%2020231108225147.png)


# 创建站点
https://app.netlify.com/teams/fb208/overview

1. Digital Garden一键部署
https://app.netlify.com/start/deploy?repository=https://github.com/oleeskild/digitalgarden
![assets/Pasted image 20231108225904.png](/img/user/assets/Pasted%20image%2020231108225904.png)
链接Github、填写仓库名，会自动创建仓库