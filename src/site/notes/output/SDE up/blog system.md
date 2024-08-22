---
{"dg-publish":true,"dg-permalink":"SDE/2024/blog-system","permalink":"/SDE/2024/blog-system/","title":"Blog System","tags":["obsidian","blog","digital-garden"],"dgHomeLink":true,"dgShowBacklinks":true,"dgShowFileTree":true,"dgEnableSearch":true,"dgShowToc":true,"dgShowTags":true}
---



如何处理 

内容带代码的

类似于sql注入


需求经历：
1. 最初的想法建造一个 github的同名仓库，看起来很炫 => 为了展示个人网站 找工作需要
2. 然后是不一定是同名仓库，根源是一个自己的博客网站，找到一个静态网页生成的东西，如hexo等 => 发现自己的blog主要存在于 obsidian上
3. 目前是如何针对现有的文本编辑器 obsidian进行发布操作 并且可以添加个人的CV（一个好看的markdown而已）
		为什么不直接使用notion: 网页编辑太慢 + 需要联网 + 添加个人CV效果不佳
	最好 任意设备操作方便 

核心需求
并且需要频繁的更新
如何把自己的obsidian 本地的markdown文件 => 直接变成互联网可见的blog system

附加需求：123
如何顺便展示CV


查找解决方案: 
1. Google 中文
2. Google 英文
3. Obsidian 插件市场查找 
4. b站中文查找 obsidian 博客


a template 
https://notes.thatother.dev/



Digital Garden

Header Date Format should not add the specific time 



Error when publishing multiple ones

报错不一定管用
看看报错前面的内容  要使用 permalink=> https://dg-docs.ole.dev/advanced/note-specific-settings/




![blog system-20240816140142445.webp](/img/user/output/SDE%20up/attachments/blog%20system-20240816140142445.webp)


=> 问题很神奇  
因为打开的文件base不一样 
在上一层文件夹里面打开，相对地址就不对了 

=> 如何在static generator里面更改东西呢 
别人在每个生成页面都是写好了的，全是静态页面
![blog system-20240816142905684.webp](/img/user/output/SDE%20up/attachments/blog%20system-20240816142905684.webp)




=> 最终解决方案

https://github.com/oleeskild/obsidian-digital-garden

obsidian+vercel+自己的域名
obsidian存储的东西还是本地的，不需要图床！

本地完全编辑后 
安装digital garden 插件
并且根据   https://dg-docs.ole.dev/getting-started/01-getting-started/   一步一步完成
最后购买squarespace域名，connecting to vercel 
