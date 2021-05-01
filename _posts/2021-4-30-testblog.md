---
layout: post
title: docker真的太好用了吧
---

昨天晚上搞了一晚上ruby，jekyll就是装不上去，然后后来我把所有的gem源都删了，发现安不上源了，老崩溃了。突然想到我干嘛不用docker，根本不需要安这些幺蛾子，打开docker，打开教程，
```
run -it --rm -v "$PWD":/usr/src/app -p "4000:4000" starefossen/github-pages
```
一行代码在文件夹console一输（含有那个jekyll配置_config.yaml的路径哦），浏览器打开，127.0.0.1：4000妥妥的，爱了爱了

*ps:pwd报错的话，直接改成绝对路径哈*
```
docker run -it --rm -v "D:\study\code\xuerongchuan.github.io":/usr/src/app -p "4000:4000" starefossen/github-pages
```
*ps:[亲爱的教程](https://www.jamessturtevant.com/posts/Running-Jekyll-in-Windows-using-Docker/),docker你值得拥有*
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTc3OTQ1NjQ5MF19
-->