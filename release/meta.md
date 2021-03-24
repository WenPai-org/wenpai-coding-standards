---
layout: default
title: 元信息编写规范
nav_order: 1
parent: 应用发布规范
---

# 元信息编写规范

## 前言

WordPress通过元信息来读取并分析插件、主题的基本信息，并作为将其装载的依据。本章节分别给出对插件及主题元信息的编写示范。

## 插件元信息

插件元信息包含以下字段

 * Plugin Name: （必需）插件的名称，它将显示在WordPress管理后台的插件列表中
 * Plugin URI: 插件的发布页面，不要填写wordpress.org上的发布地址，而应该是自己的独立域名
 * Description: 插件的简短描述。字数限制为70
 * Version: 插件的当前版本号，如1.0或1.0.3
 * Requires at least: 插件兼容的最低WordPress版本
 * Requires PHP: 所需的最低PHP版本
 * Author: 插件作者的姓名。可以使用逗号列出多个作者
 * Author URI: 作者的主页
 * License: 插件许可证的短名称，文派系列项目统一使用：GPLv2
 * License URI: 指向许可证全文的链接，文派系列项目统一使用：https://www.gnu.org/licenses/gpl-2.0.html
 * Text Domain: 插件的gettext文本域
 * Domain Path: 插件的翻译文件存储路径
 * Network: 多站点环境下插件是否只能全局激活。如果是，请设置值为true，不需要时应忽略

以下是一个通常的示范，若无特殊要求可直接复制

```
/**
 * Plugin Name: 插件名
 * Plugin URI: 插件发布页
 * Description: 插件描述
 * Author: WenPai.org
 * Author URI: https://wenpai.org/
 * Version: 插件版本号
 * License: GPLv2
 * License URI: http://www.gnu.org/licenses/gpl-2.0.html
 */
```
