---
{"dg-publish":true,"permalink":"/15-知识星球/SUNO-AI中文手册/6-技巧/2-灵活运用Extend功能/","dgPassFrontmatter":true,"created":"2024-07-30T22:39:36.953+08:00","updated":"2024-07-31T23:08:09.028+08:00"}
---


# 创造风格相似的歌
在sunoAI中，及时使用相同的配置，也会生成完全风格迥异的歌曲，
如果你很喜欢某一首歌的风格，可以利用“Extend”功能来生成相似风格的歌曲。
![](/img/user/assets/Pasted image 20240730152920.png)
可以在这里截取歌曲的前几秒，让他重新生成，这样即使更换了歌词，生成的新歌风格也会和之前的非常相似。

# 将纯音乐转换为完整的歌曲

生成的纯音乐，也可以利用“Extend”功能来重新填入歌词，只需要保留原音乐10秒-20秒，然后再加入歌词即可。
## 实例
曲
<audio controls>
  <source src="https://123.markup.com.cn/ibed/202407300948694.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

填词后
<audio controls>
  <source src="https://123.markup.com.cn/ibed/202407300948398.mp3" type="audio/mpeg">
  Your browser does not support the audio element.
</audio>

# 解决Extend两段歌曲不连贯问题
假如你给的歌词太长，4分钟内唱不完，当你扩展时不要从4分钟开始，而是从前一个段落的时间节点，比如：
![](/img/user/assets/Pasted image 20240730154259.png)
上图中4分钟断在了红色位置，但是你扩展歌曲时，选择从3:50处进行扩展才是最好的选择。
