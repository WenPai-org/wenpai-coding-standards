---
layout: default
title: readme.txt文件编写规范
nav_order: 1
parent: 应用发布规范
---

# readme.txt文件编写规范

## 前言

wordpress.org的仓库通过插件及主题根目录下的readme.txt文件读取并向用户展示插件描述、截图信息。

## 插件readme.txt

以下给出一个范本，你只需要填充内容 or 删除不需要的区块即可。

``` 
=== 插件名称 ===
Tags: 插件标签，用英文逗号分割
Requires at least: 需要的最低WordPress版本
Tested up to: 经过测试的最高WordPress版本
Requires PHP: 需要的最低PHP版本
Stable tag: 插件版本号
License: GPLv2
License URI: http://www.gnu.org/licenses/gpl-2.0.html

插件短描述，70个字内。

== Description ==

插件长描述

== Installation ==

插件安装、初始化方法

== Screenshots ==

插件截图的标题，类似如下所示，每一行对应SVN中的一张截图，使用序号区分：
1. 这一行对应screenshot-1.(png|jpg)
2. 这一行对应screenshot-2.(png|jpg)
3. 这一行对应screenshot-3.(png|jpg)

== Changelog ==

这里是变更日志

### 版本号 ###

* 更新内容一
* 更新内容二
* 更新内容三

```
