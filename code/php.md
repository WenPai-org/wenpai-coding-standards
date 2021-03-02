---
layout: default
title: PHP编码规范
nav_order: 1
parent: 编码规范
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

if\|else if与if\|elseif冒号语法不兼容。因此，请使用if\|elseif。

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

## 多行函数调用

将函数调用拆分为多行时，每个参数必须位于单独的行上。单行内联注释可以占用其自己的行。

每个参数最多只能占用一行。必须将多行参数值分配给一个变量，然后将该变量传递给函数调用。

```php 
$bar = array(
    'use_this' => true,
    'meta_key' => 'field_name',
);
$baz = sprintf(
    /* translators: %s: Friend's name */
    esc_html__( 'Hello, %s!', 'yourtextdomain' ),
    $friend_name
);
 
$a = foo(
    $bar,
    $baz,
    /* translators: %s: cat */
    sprintf( __( 'The best pet is a %s.' ), 'cat' )
);
```

## 正则表达式

应优先使用与Perl兼容的正则表达式（[PCRE](http://php.net/pcre)，`preg_` 函数），而不要使用与之对应的POSIX风格。切勿使用/e开关，而应使用preg_replace_callback。

将单引号字符串用于正则表达式是最方便的，因为与双引号字符串相反，它们只有两个元序列：\'和\\。

## 打开和关闭PHP标签

在HTML块中嵌入多行PHP代码段时，PHP打开和关闭标签必须单独位于一行上。

正确（多行）：

```php 
function foo() {
    ?>
        <div>
        <?php
        echo bar(
            $baz,
            $bat
        );
        ?>
        </div>
    <?php
}
```

正确（单行）：

```php 
<input name="<?php echo esc_attr( $name ); ?>" />
```

错误：

```php 
if ( $a === $b ) { ?>
<some html>
<?php }
```

## 切勿使用简写PHP开始标记

正确：

```php 
<?php ... ?>
<?php echo $var; ?>
```

错误：

```php 
<? ... ?>
<?= $var ?>
```

## 删除尾随空格

在每行代码的末尾删除尾随空格。最好省略文件末尾的PHP结束标记。如果使用标记，请确保删除尾随空格。

## 空格的使用

始终在逗号后以及逻辑，比较，字符串和赋值运算符的两侧放置空格。

```php 
x === 23
foo && bar
! foo
array( 1, 2, 3 )
$baz . '-5'
$term .= 'X'
```

在if、elseif、foreach、for和switch块的左括号和右括号两边加空格。

```php 
foreach ( $foo as $bar ) { ...
```

定义函数时：

```php 
function my_function( $param1 = 'foo', $param2 = 'bar' ) { ...
 
function my_other_function() { ...
```

调用函数时：

```php 
my_function( $param1, func_param( $param2 ) );
my_other_function();
```

执行逻辑比较时：

```php 
if ( ! $foo ) { ...
```

类型强制转换必须为小写。总是使用类型转换的缩写形式，(int)而不是(integer)、(bool)而不是(boolean)。对于浮点型使用(float)：

```php 
foreach ( (array) $foo as $bar ) { ...
 
$foo = (bool) $bar;
```

引用数组项时，如果索引是变量，则仅在索引周围包含空格，例如：

```php 
$x = $foo['bar']; // 正确
$x = $foo[ 'bar' ]; // 错误
 
$x = $foo[0]; // 正确
$x = $foo[ 0 ]; // 错误
 
$x = $foo[ $bar ]; // 正确
$x = $foo[$bar]; // 错误
```

在switch块中，case语句的冒号前不能有空格。

```php 
switch ( $foo ) {
    case 'bar': // 正确
    case 'ba' : // 错误
}
```

类似地，在返回类型声明的冒号之前不应该有空格。

```php 
function sum( $a, $b ): float {
    return $a + $b;
}
```

除非另有规定，否则括号内应该有空格。

```php 
if ( $foo && ( $bar || $baz ) ) { ...
 
my_function( ( $x - 1 ) * 5, $y );
```

## 格式化SQL语句

在格式化SQL语句时，你可以将它分成几行，如果它足够复杂，可以进行缩进。不过，大多数语句都是一行的。始终将语句的SQL部分大写，如UPDATE或WHERE。

用于更新数据库的函数在传参时不应对SQL进行斜杠转义。转义操作应该尽可能在SQL执行时进行，同时最好使用$wpdb->prepare()进行转义处理。

$wpdb->prepare()是一种处理SQL查询的转义、引用和内部转换的方法。它使用sprintf()对数据处理。例子：

```php 
$var = "dangerous'"; // 可能需要转义的原始数据
$id = some_foo_number(); // 期望这是整数数据，但是不能确定
 
$wpdb->query( $wpdb->prepare( "UPDATE $wpdb->posts SET post_title = %s WHERE ID = %d", $var, $id ) );
```

%s用于字符串占位符，%d用于整数占位符。请注意，它们不仅仅只是“引用”！$wpdb->prepare()将为我们进行转义和引用。这样做的好处是，我们不必记住手动使用esc_sql()，而且一目了然。

## 数据库查询

避免直接接触数据库。如果有一个已定义的函数可以获取所需的数据，请使用它。数据库抽象（使用函数而不是查询）有助于保持代码向前兼容，并且在结果缓存在内存中的情况下，它可以快很多倍。

## 命名约定

在变量、action/filter和函数名称中使用小写字母（切勿使用camelCase）。通过下划线将单词分开。不要不必要地缩写变量名，让代码明确且具有自我说明性。

```php 
function some_name( $some_variable ) { [...] }
```

类名应使用下划线分隔的大写单词。任何首字母缩写词都应全部大写。

```php 
class Walker_Category extends Walker { [...] }
class WP_HTTP { [...] }
```

常量应全部大写，并用下划线分隔单词：

```php 
define( 'DOING_AJAX', true );
```

文件应使用小写字母描述性地命名。使用连字符分隔单词。

```php 
my-plugin-name.php
```

类文件名应基于带有类前缀的类名，并且类名中的下划线用连字符代替，例如WP_Error变为：

```php 
class-wp-error.php
```

该文件命名标准适用于所有具有类的当前文件和新文件。

## 函数参数的自解释标志值

调用函数时，应优先使用字符串值而不是true或false。

```php 
// 错误
function eat( $what, $slowly = true ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', true ); // 真正的意思是什么？
eat( 'dogfood', false ); // false是什么意思？
```

由于PHP不支持命名参数，因此标志的值没有意义，并且每次遇到上述示例的函数调用时，我们都必须搜索函数定义。 通过使用描述性字符串值而不是布尔值，可以使代码更具可读性。

```php 
// 正确
function eat( $what, $speed = 'slowly' ) {
...
}
eat( 'mushrooms' );
eat( 'mushrooms', 'slowly' );
eat( 'dogfood', 'quickly' );
```

当需要更多的单词来描述函数参数时，$args数组可能是更好的模式。

```php 
// 更好的
function eat( $what, $args ) {
...
}
eat ( 'noodles', array( 'speed' => 'moderate' ) );
```

## 使用插值方式命名动态挂钩

动态挂钩应该使用内插而不是串联来命名，以提高可读性和可发现性。

动态挂钩是在其标记名称中包含动态值的挂钩，例如：

```php 
{$new_status}_{$post->post_type} (publish_post).
```

挂钩标签中使用的变量应使用大括号{}括起来，完整的外部标签名称应使用双引号引起来。这是为了确保PHP可以正确解析字符串中插入的变量。

```php 
do_action( "{$new_status}_{$post->post_type}", $post->ID, $post );
```

在可能的情况下，标记名称中的动态值也应尽可能简洁明了。$user_id比$this->id更能自我描述。

## 三元运算符

[三元运算符](http://en.wikipedia.org/wiki/Ternary_operation)很好，但是始终让它们测试语句是否为真，而不是假。否则，它将变得混乱。（一个例外是使用!empty()，因为这里测试false通常更直观。）

不得使用短三元运算符。

例如：

```php 
// (if statement is true) ? (do this) : (else, do this);
$musictype = ( 'jazz' === $music ) ? 'cool' : 'blah';
// (if field is not empty ) ? (do this) : (else, do this);
```

## 尤达条件式

```php 
if ( true === $the_force ) {
    $victorious = you_will( $be );
}
```

在进行变量的逻辑比较时，始终将变量放在右侧，将常量或函数调用放在左侧。如果双方都不是变量，则顺序并不重要。

在上面的示例中，如果省略了等号，你会收到解析错误，因为你无法为诸如true的常量赋值另一个常量。如果该语句与之相反（$the_force = true），则该分配将完全有效，返回1，导致if语句的计算结果为true，你可能会为此浪费大量时间排错。

这适用于==、!=、===和!==。<>、<=或>=的尤达条件式明显很难阅读，所以不对于这些表达式不使用尤达条件式。

## 聪明的代码

通常，可读性比巧妙或简短更为重要。

```php 
isset( $var ) || $var = some_function();
```

尽管以上内容很巧妙，但如果你不熟悉，可能还需要花费一些时间去理解。因此，最好这样写：

```php 
if ( ! isset( $var ) ) {
    $var = some_function();
}
```

除非绝对必要，否则不应该使用松散比较，因为它们的结果可能会误导你（松散比较将会对右侧类型强制转换为左侧类型，比如0转"0"）。

正确：

```php 
if ( 0 === strpos( '文派', 'foo' ) ) {
    echo __( '你好文派！' );
}
```

错误：
```php 
if ( 0 == strpos( '文派', 'foo' ) ) {
    echo __( '你好文派！' );
}
```

不得将赋值语句放在条件中。

正确：

```php 
$data = $wpdb->get_var( '...' );
if ( $data ) {
    // Use $data
}
```

错误：

```php 
if ( $data = $wpdb->get_var( '...' ) ) {
    // Use $data
}
```

在switch语句中，可以有多个空的case落入一个公共代码块。但如果一个case包含一个代码块却没有break而是进入下一个case，则必须对此进行明确注释。

```php 
switch ( $foo ) {
    case 'bar':       // 正确，一个空的case，可以不加任何注释，直接落空。
    case 'baz':
        echo $foo;    // 错误，带有代码块的case如果不加break就必须添加明确注释。
    case 'cat':
        echo 'mouse';
        break;        // 正确，含有break时不需要评论。
    case 'dog':
        echo 'horse';
        // no break   // 正确，一个case可以注释以明确提及不break的原因。
    case 'fish':
        echo 'bird';
        break;
}
```

永远不要使用goto语句。

eval()构造非常危险，并且无法保护。此外，PHP 7.2中不推荐使用内部执行eval()的create_function()函数。这两个都不能使用。
