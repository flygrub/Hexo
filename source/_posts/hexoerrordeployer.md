title: hexo ERROR Deployer not found
date: 2016-01-19 13:51:09
tags: hexo
---

执行发布指令 hexo d 的时候报错

> hexo ERROR Deployer not found: github

## 解决方法：

> * 执行命令
``` bash
npm install hexo-deployer-git --save
```
> *  将deploy 的 type由github改为git