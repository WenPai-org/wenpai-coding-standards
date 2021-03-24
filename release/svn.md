---
layout: default
title: WordPress SVN发布规范
nav_order: 3
parent: 应用发布规范
---

# WordPress SVN发布规范

## 前言

wordpress.org的应用仓库使用SVN托管，所以发布时需要使用SVN客户端对代码进行推送，本章节罗列了一些规范约定和注意事项。

## 规范约定

### 1. 提交日志

 (1). 若当前提交的内容是发布了一个新版本，则提交日志内容为：v版本号，例如：v1.0.0
 (2). 若当前提交的内容是更新了一个显示在wordpress.org目录的静态资源，例如截图、图标、横幅，则提交日志内容为：update assets

## 注意事项

### 1. 不要直接使用trunk目录推送更新 
    
正确做法应该是复制trunk目录中的代码到`tag/版本号`目录中进行发布，虽然wordpress.org的系统允许使用trunk发布，但也不应该这样做。因为这样的话会无法追踪应用的历史版本。
