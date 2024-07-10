---
title: 'Delpoy_site'
date: 2024-07-10T14:17:50+08:00
keywords: ["", ""]
cover: ""
summary: "how to deploy the site"
tags: ["github—pages", "blog"]
pin: false
lastmod: true
draft: false
hero: ""
title: ''
menu:
  sidebar:
    name: Delpoy_site
    identifier: 
    parent: 
    weight: 500
---
# 1. L/O文件目录

## 1.1 Local 文件目录

```python
目前使用的文件根是blog/
其下存在两个root directory
* eric-blog是github-style的blog
* toha_style是toha-style的blog
```

## 1.2 Origin 分支解释

```python
目前origin存在两个branch
main: github-style的blog, root is ~/blog/eric_blog/public
eric-blog-toha-style: toha-style的blog, root is ~/blog/toha_style/
```

----



# 2. Pages & Actions & Deployment

![avatar](images/posts/bgd.jpg)

GitHub Pages 仓库：储存由 Hugo 从Markdown 文件生成的 HTML 文件；用于一组静态网页集合（Static Web Page），这些静态网页由 GitHub 托管（host）和发布，所以是 GitHub + Pages

repo：https://github.com/eirctong/eirctong.github.io

web_source：https://erictonger.github.io/

## 2.1 博客源仓库

博客源仓库：储存所有 Markdown 源文件（博客内容），和博客中用到的图片等https://github.com/eirctong/blog

```python
目前仅一个main分支，存储的是所有eric-blog的/内容
# TODO 新增一个分支 用于存储所有的toha_style的内容
```

## 2.2 deployment

在.github/workflows下创建deploy-site.yaml

https://github.com/erictonger/erictonger.github.io/blob/eric-blog-toha-style/.github/workflows/deploy-site.yaml



# 3. 全流程

```python
# 进入blog的路径 启动hugo server 在本地检查markdown转换成html有无问题
cd specific dict
hugo server -w # try http://localhost:1313/
hugo new post/xxx.md



# push所有内容到博客源仓库,用于远程备份 此步暂时不需要
# 如果没有把博客源文件推送到远端仓库备份，假设你丢失了本地文件（比如电脑坏了），那只根据public文件夹中的内容是很难复原你的所有博客内容的
cd blog
git stash -u
git pull --rebase origin main
git stash pop
git add .
git commit -m "feat(init deploy): test publish"
git push


# 向GitHub Pages 推送, 静态网页资源将会Automatic Action
hugo # 通过 hugo 命令生成静态网页文件,会修改toml中的baseurl所关联的html中的item
cd toha_style/
git stash -u
git pull --rebase origin main
git stash pop
git add .
git commit -m "feat(update blog): push blog"
git push
# try https://eirctong.github.io/
```
