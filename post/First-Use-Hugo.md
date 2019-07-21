---
title: "First Use Hugo"
date: 2019-07-21T14:29:43+08:00
lastmod: 2019-07-21T14:29:43+08:00
draft: true
keywords: []
description: "第一次使用hugo"
tags: ["Django"]
categories: ["python"]
author: "herschel"

# You can also close(false) or open(true) something for this content.
# P.S. comment can only be closed
comment: false
toc: false
autoCollapseToc: false
postMetaInFooter: false
hiddenFromHomePage: false
# You can also define another contentCopyright. e.g. contentCopyright: "This is another copyright."
contentCopyright: false
reward: false
mathjax: false
mathjaxEnableSingleDollar: false
mathjaxEnableAutoNumber: false

# You unlisted posts you might want not want the header or footer to show
hideHeaderAndFooter: false

# You can enable or disable out-of-date content warning for individual post.
# Comment this out to use the global config.
#enableOutdatedInfoWarning: false

flowchartDiagrams:
  enable: false
  options: ""

sequenceDiagrams: 
  enable: false
  options: ""

---

<!--more-->
# Hugo使用
- 虽然这是配置比较简单的静态博客,但还是折腾了我半天，感谢CodeSheep的示范
## 下载Hugo
- 我使用的是Ubuntu18.04，`sudo apt-get upgrade`
- `sudo apt-get install hugo`
- `hugo version`
- 简单几步就可以下载使用Hugo了
## 创建博客文件
- 一般来说此博客文件是Hugo的Blog以后主要依赖文件，一定要用命令去创建
- `hugo new site myblog`
## 挑选主题
- 先准备好git
- [官网](theme.hugo.io)挑选好自己的主题，最看看ReadMe
- 克隆下载(我的是light-hugo主题)
## 本地启动
- `hugo -t light-hugo --buildDrafts`或者`hugo -t light-hugo -D`
## 真正写一篇博文
- `hugo new post/xxx.md`
- 一定要在post下，血的教训
## 部署到白嫖的github上面
- 在github上面以自己的昵称create a new empty repository
  - *要注意的是在这里名称一定不能写错，博主就是在建仓库的时候名称缺写了个c从而出现问题，登录xxx.github.io无效*
- `hugo --theme=light-hugo --baseUrl="https://herschel-ma.github.io/" --buildDrafts`
  - *这里同样不能少最后的--，博主就是粗心少了，导致访问出现404，最后还得版本回退，pull -rebase origin master,再push*
- 到这里基本就大功告成了，接着就是熟悉的git提交了
- 上一步之后出现了public文件夹
  - `cd public/`
- `git init`
- `git add .`
- `git commit -m "我的Hugo第一次测试提交"`
- `git remote add origin https://github.com/herschel-ma/herschel-ma.github.io.git`
- `git push -u origin master`
## 总结
- 这样就完成了Hugo博客的搭建，虽然有点丑，但本篇重在部署，之后再进行优化吧