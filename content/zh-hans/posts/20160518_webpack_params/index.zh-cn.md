---

date: 2016-05-17
title: webpack相关命令参数
tags: 
  - webpack
category:
  - 工具
slug: WebpackParams
---
使用webpack的同学，你真的熟练应用webpack么？其实直接`webpack --help`就可以看到，但是有很多人还是在网上到处搜相关参数
<!--more-->
### 一、webpack相关参数
```
  webpack --config webpack.config.dev.js   //使用另一份配置文件（比如webpack.config2.js）来打包
  webpack --watch   //监听变动并自动打包
  webpack //执行一次开发时的编译
  webpack -p //执行一次生成环境的编译（压缩）
  webpack --watch //在开发时持续监控增量编译（很快）
  webpack -d //让他生成SourceMaps
  webpack --progress //显示编译进度
  webpack --colors //显示静态资源的颜色
  webpack --sort-modules-by, --sort-chunks-by, --sort-assets-by //将modules/chunks/assets进行列表排序
  webpack --display-chunks //展示编译后的分块
  webpack --display-reasons //显示更多引用模块原因
  webapck --display-error-details //显示更多报错信息
```
