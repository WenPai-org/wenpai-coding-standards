---
layout: default
title: PHP编码规范
nav_order: 1
parent: WordPress官方编码规范
---

# PHP编码规范

## 单引号与双引号

如果你无需解析字符串中的变量，请使用单引号。你几乎永远都不必转义字符串中的引号，因为你可以仅通过更改引号样式来避免冲突，如下所示：

```php 
echo '<a href="/static/link" title="Yeah yeah!">Link name</a>';
echo "<a href='$link' title='$linktitle'>$linkname</a>";
```

嵌入文本的变量应该始终经过esc_attr()消毒，以便正确转义其中的符号，防止造成HTML意外结束等安全问题，更多信息参考：[数据消毒](https://codex.wordpress.org/Data_Validation)

## 缩进

代码缩进应始终反映逻辑结构。使用tab而不要使用空格，因为这样可以最大程度上的兼容不同的客户端。

例外情况：对于一些代码，如果对齐会使其更易读，则这种情况下应使用空格：

```php 
[tab]$foo   = 'somevalue';
[tab]$foo2  = 'somevalue2';
[tab]$foo34 = 'somevalue3';
[tab]$foo5  = 'somevalue4';
```

对于数组，当其中包含多个元素时，每个元素都应从新行开始：

只包含一个元素：

```php 
$query = new WP_Query( array( 'ID' => 123 ) ); 
```

包含多个元素：

```php 
$args = array(
[tab]'post_type'   => 'page',
[tab]'post_author' => 123,
[tab]'post_status' => 'publish',
);

$query = new WP_Query( $args );
```

注意最后一个数组项后面的逗号：建议这样做，因为这样可以更轻松地更改数组的顺序，并在添加新元素时使差异更清晰。

```php 
$my_array = array(
[tab]'foo'   => 'somevalue',
[tab]'foo2'  => 'somevalue2',
[tab]'foo3'  => 'somevalue3',
[tab]'foo34' => 'somevalue3',
);
```

对于switch结构，case应在switch语句中缩进一个tab，并在case语句中缩进一个tab输入break。

```php 
switch ( $type ) {
[tab]case 'foo':
[tab][tab]some_function();
[tab][tab]break;
[tab]case 'bar':
[tab][tab]some_function();
[tab][tab]break;
}
```

> PS：应在行的开头使用tab来缩进，而在行的中间则可以使用空格来对齐。
