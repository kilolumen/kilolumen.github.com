---
title: Install Hexo
date: 2018-10-10 16:59:50
categories:
tags:
---

<!--more-->

### 安装前提

* Git
* Node.js

### 安装 Hexo

安装

```
$ npm install -g hexo-cli
$ npm install hexo --save
```

部署Hexo
```
hexo init <folder>
cd <folder>
npm install
```

安装Hexo 插件
```
npm install hexo-generator-index --save
npm install hexo-generator-archive --save
npm install hexo-generator-category --save
npm install hexo-generator-tag --save
npm install hexo-server --save
npm install hexo-deployer-git --save
npm install hexo-deployer-heroku --save
npm install hexo-deployer-rsync --save
npm install hexo-deployer-openshift --save
npm install hexo-renderer-marked@0.2 --save
npm install hexo-renderer-stylus@0.2 --save
npm install hexo-generator-feed@1 --save
npm install hexo-generator-sitemap@1 --save
```


