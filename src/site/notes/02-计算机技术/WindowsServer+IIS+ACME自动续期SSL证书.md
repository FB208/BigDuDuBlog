---
{"dg-publish":true,"permalink":"/02-计算机技术/WindowsServer+IIS+ACME自动续期SSL证书/","dgPassFrontmatter":true,"created":"2024-01-22T11:45:34.479+08:00","updated":"2024-01-22T21:44:41.000+08:00"}
---

#WindowsServer #LetsEncrypt #iis  #ssl证书续期 #https

基与Windows系统的Let’s Encrypt证书自动续期，免去整天盯着续期和替换证书的烦恼

# IIS开启反向代理（非必须）
此操作非必须，因为的是用IIS代理的java服务的地址，所以必须开启此功能，如果是静态网站或者.net项目，不需要这一步

首先安装两个IIS插件：Application Request Routing(ARRv3.0)、Url-Rewite
**官网下载**
[URL Rewrite : The Official Microsoft IIS Site](https://www.iis.net/downloads/microsoft/url-rewrite)
[Application Request Routing : The Official Microsoft IIS Site](https://www.iis.net/downloads/microsoft/application-request-routing)
**备份下载**
http://back.kod.bigdudu.cn:6688/#s/-D3n_SRQ


安装完ARR之后IIS中功能菜单中会多出一个Application Request Routing Cache中，打开代理的功能。
![assets/Pasted image 20240122142718.png](/img/user/assets/Pasted%20image%2020240122142718.png)
![assets/Pasted image 20240122142735.png](/img/user/assets/Pasted%20image%2020240122142735.png)
![assets/Pasted image 20240122142744.png](/img/user/assets/Pasted%20image%2020240122142744.png)
URL ReWrite安装后，点开一个网站功能菜单中会有一个 URL 重写，根据安装的版本不通URL 重写有可能是中文版或是英文版，下载的时候注意选择对应的版本。
![assets/Pasted image 20240122142826.png](/img/user/assets/Pasted%20image%2020240122142826.png)
接下来就可以进行IIS的方向代理配置，这里我们新建一个网站叫ProxyTest，绑定域名 [http://proxy.niubi.com](https://link.zhihu.com/?target=http%3A//proxy.niubi.com)（这个换成自己需要的域名）。
![assets/Pasted image 20240122142854.png](/img/user/assets/Pasted%20image%2020240122142854.png)
配置完成后访问一下网站
![assets/Pasted image 20240122142847.png](/img/user/assets/Pasted%20image%2020240122142847.png)

接下来配置方向代理，打开proxytest网站的URL重写，点击添加规则，点击弹出页面的空白规则。创建如下配置，点击应用。模式选择与模式匹配，模式的正则表达式填写(.*)全部匹配。

> 注意重写URL后面一定要加/{R:1}，否则只能代理一级路径
> 

![assets/Pasted image 20240122142907.png](/img/user/assets/Pasted%20image%2020240122142907.png)

重写的URL填写你本地的一个URL，或者局域往内的URL，这样就能够实现返现代理的功能，浏览器中打开 [http://proxy.niubi.com](https://link.zhihu.com/?target=http%3A//proxy.niubi.com)，就可以访问到本机的8089下的内容。
![assets/Pasted image 20240122143019.png](/img/user/assets/Pasted%20image%2020240122143019.png)

配置完成后知道刚刚proxy网站的目录下，可以看到一个web.config文件，反向代理的设置就配置在这个web.config中。
![assets/Pasted image 20240122143036.png](/img/user/assets/Pasted%20image%2020240122143036.png)
# win-acme创建和续期证书

github： https://github.com/win-acme/win-acme

下载**win-acme**并解压，得到下面的文件
![](https://qiniu.bigdudu.cn/202401221346786.png)

通过IIS配置站点，添加主机名，配置后确认通过HTTP可以访问，具体过程这里不赘述
下图中的配置就要保证 http://bigdudu.cn:6666 是可以访问的
![assets/Pasted image 20240122152916.png](/img/user/assets/Pasted%20image%2020240122152916.png)

运行wacs
![assets/Pasted image 20240122152616.png](/img/user/assets/Pasted%20image%2020240122152616.png)


输入N，然后输入要创建证书的网站序号1
![assets/Pasted image 20240122153114.png](/img/user/assets/Pasted%20image%2020240122153114.png)

选择A
![assets/Pasted image 20240122153204.png](/img/user/assets/Pasted%20image%2020240122153204.png)

后面就是一路“y”，注意输入邮箱这一步，输入可用的邮箱
![assets/Pasted image 20240122153250.png](/img/user/assets/Pasted%20image%2020240122153250.png)

执行成功，证书已经完成创建，回到IIS看绑定，ssl证书已经自动绑定好了
我用的是v2.2.5.1541.x64.pluggable版本，默认自动续期
![assets/Pasted image 20240122153424.png](/img/user/assets/Pasted%20image%2020240122153424.png)


# 使用domian-admin监控SSL证书状态


<div class="transclusion internal-embed is-loaded"><div class="markdown-embed">




用于监控ssl证书过期问题，并通过多种方式提醒

官方文档： https://domain-admin.readthedocs.io/zh-cn/latest/manual/install.html#docker
GitHub： https://github.com/mouday/domain-admin

``` shell
docker run \
-d \
-v /home/DockerVolumes/domain-admin/database:/app/database \
-v /home/DockerVolumes/domain-admin/logs:/app/logs \
-p 8000:8000 \
--name domain-admin \
mouday/domain-admin:latest
```

> 默认账号密码 admin  /  123456


# 配置邮件提醒

## 配置发件服务器信息
![assets/Pasted image 20240122162438.png](/img/user/assets/Pasted%20image%2020240122162438.png) ![assets/Pasted image 20240122162609.png](/img/user/assets/Pasted%20image%2020240122162609.png)

## 测试发送邮件
注意要把剩余天数调高，如已经配置了一个证书100天到期，剩余天数就要设置101天，才能发送邮件
![assets/Pasted image 20240122162640.png](/img/user/assets/Pasted%20image%2020240122162640.png)

邮件样式
![assets/Pasted image 20240122162744.png](/img/user/assets/Pasted%20image%2020240122162744.png)

</div></div>
