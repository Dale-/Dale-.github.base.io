---
title: Hexo + Github + Blog
layout: post
date: 2015-10-23 17:48:53
comments: true
tags: [Hexo]
categories: [Hexo]
keywords: Hexo Github
description: build hexo blog quickly
---

## 快速搭建Blog

### 1. 安装hexo

``` bash
npm install -g hexo
```

### 2. 初始化hexo

安装完成后，创建一个存放你Blog的文件夹，进入到该文件夹下，执行hexo init

Hexo即会自动在目标文件夹下建立网站所需要的所有文件

``` bash
hexo init
```

<!-- more -->

### 3. 安装依赖

``` bash
npm install
```

### 4. 启动服务测试

``` bash
hexo s
```

用浏览器打开http://localhost:4000/或者http://127.0.0.1:4000/就可以看到页面了

默认会有一个Hello World 的文章

### 5. 关联Github

  * 进入Github建立一个[ID].github.io的Repository

  ![](/images/hexo-github/createRepository.png)

  * 关联你所新建的Github repository

  打开你hexo目录下的_config.yml文件，找到最后配置deploy

  按照下图的三项配置你的deploy信息

  ![](/images/hexo-github/config.png)

### 6. 部署

``` bash
hexo d
```  

deploy之后就可以打开[yourname].github.io看到你的Blog了

## Hexo常用命令

``` bash
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #将.deploy目录部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
hexo deploy -g  #生成加部署
hexo server -g  #生成加预览
命令的简写
hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy
```  

## 图片deploy后找不到

![](/images/hexo-github/localImgPath.png)

![](/images/hexo-github/remoteImgPath.png)

引用图片时，注意deploy到github后的路径

> 如果将图片放到public中，hexo clean的时候会清空，所以建议将img放到source中。

## 引用Theme

 * 选择你中意的theme，clone到themes目录下
 * 在_config.yml文件中找到下图的配置项，修改theme
 ![](/images/hexo-github/configTheme.png)
 * 如果你下载的主题以hexo-theme-X命名，在改变配置项的theme前，重命名主题为X
 * 如果你下载的主题的配置项文件以example结尾，请删除掉example

## Markdown Preview
 用Atom打开你的markdown文件，Ctrl + Shift + M
 ![](/images/hexo-github/markdownPreview.png)
