---
layout: default
title: PHP编码规范
nav_order: 2
parent: WordPress官方编码规范
permalink: /wordpress
---

# PHP

## 单引号与双引号

如果你无需解析字符串中的变量，请使用单引号。你几乎永远都不必转义字符串中的引号，因为您可以仅更改引号样式，如下所示：

```php
echo '<a href="/static/link" title="Yeah yeah!">Link name</a>';
echo "<a href='$link' title='$linktitle'>$linkname</a>";
```