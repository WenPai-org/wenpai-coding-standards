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

PS：应在行的开头使用tab来缩进，而在行的中间则可以使用空格来对齐。

## 大括号样式

大括号应按以下所示样式用于所有代码：

```php 
if ( condition ) {
    action1();
    action2();
} elseif ( condition2 && condition3 ) {
    action3();
    action4();
} else {
    defaultaction();
}
```

如果你有一个很长的代码块，请考虑是否可以将其分解为两个或更多个较短的代码块、函数或方法，以降低复杂性，提高测试的简便性并提高可读性。

你应该始终使用大括号，无论按PHP语法其是否可以省略：

```php 
if ( condition ) {
    action0();
}
 
if ( condition ) {
    action1();
} elseif ( condition2 ) {
    action2a();
    action2b();
}
 
foreach ( $items as $item ) {
    process_item( $item );
}
```

请注意，要求使用大括号仅表示禁止单语句内联控制结构。你可以随意使用[其他语法](https://www.php.net/manual/en/control-structures.alternative-syntax.php)来控制结构（例如if/endif，while/endwhile），尤其是将其嵌入HTML的模板中，例如：

```php 
<?php if ( have_posts() ) : ?>
    <div class="hfeed">
        <?php while ( have_posts() ) : the_post(); ?>
            <article id="post-<?php the_ID() ?>" class="<?php post_class() ?>">
                <!-- ... -->
            </article>
        <?php endwhile; ?>
    </div>
<?php endif; ?>
```

## 使用elseif，而不是else if

if|else if与if|elseif冒号语法不兼容。因此，请使用if|elseif。

## 声明数组

array( 1, 2, 3 )与短数组语法[ 1, 2, 3 ]相比，使用长数组语法声明数组通常更具可读性，尤其是对于那些视觉困难的人。此外，它对初学者更具描述性。

数组必须使用长数组语法声明。

## 闭包（匿名函数）

在适当的情况下，可以为回调传递一个闭包而不是创建一个新函数。例如：

```php 
$caption = preg_replace_callback(
    '/<[a-zA-Z0-9]+(?: [^<>]+>)*/',
    function ( $matches ) {
        return preg_replace( '/[\r\n\t]+/', ' ', $matches[0] );
    },
    $caption
);
```

不能将闭包作为过滤器或钩子的回调传递方式，因为它们不能被remove_action()/remove_filter()删除（有关解决此问题的建议，请参见[＃46635](https://core.trac.wordpress.org/ticket/46635)）。
