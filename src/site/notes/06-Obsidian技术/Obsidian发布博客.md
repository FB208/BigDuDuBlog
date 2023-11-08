---
{"dg-publish":true,"permalink":"/06-Obsidian技术/Obsidian发布博客/","dgPassFrontmatter":true}
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

创建完成后回到Netlify
![assets/Pasted image 20231108230323.png](/img/user/assets/Pasted%20image%2020231108230323.png)
这个地址复制下来

2. Obsidian配置
下载插件 Digital Garden，配置如下
**github配置**
![assets/Pasted image 20231108230445.png](/img/user/assets/Pasted%20image%2020231108230445.png)
**Netlify站点配置**
![assets/Pasted image 20231108230459.png](/img/user/assets/Pasted%20image%2020231108230459.png)

**笔记参数配置，默认是全关闭的，可以全打开**
![assets/Pasted image 20231108230515.png](/img/user/assets/Pasted%20image%2020231108230515.png)

**样式设置**
![assets/Pasted image 20231108230817.png](/img/user/assets/Pasted%20image%2020231108230817.png)
![assets/Pasted image 20231108230831.png](/img/user/assets/Pasted%20image%2020231108230831.png)
![assets/Pasted image 20231108230836.png](/img/user/assets/Pasted%20image%2020231108230836.png)