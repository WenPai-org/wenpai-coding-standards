---
layout: default
title: 项目结构规范
nav_order: 1
parent: 项目规范
---

# 项目结构规范

## 前言

文派系列项目使用一套通用的项目结构，这样可以最大程度上降低项目交接时理解项目代码的成本。

## 插件结构规范

### 一个通常的结构

这里列出通常的目录结构，并注明每个文件 or 目录的作用。如果你负责的项目不包括某些部分，则也无需创建对应的文件 or 目录。活学活用，莫背死书。

```
plugin
├── assets             /** 静态文件存储目录 */
│   ├── img            /** 图像存储目录 */
│   ├── css            /** CSS存储目录 */
│   └── js             /** CSS存储目录 */
├── src                /** 源文件存储目录 */
│   ├── http           /** 控制器存储目录 */
│   │   │── web        /** WEB控制器存储目录 */
│   │   └── api        /** API控制器存储目录 */
│   └── cli            /** 命令行工具，由文派开发框架读取并向WordPress核心注册 */
├── tpl                /** 模板存储目录 */
├── test               /** 单元测试存储目录 */
├── vendor             /** Composer依赖存储目录 */
├── lib                /** 不在Composer存储库中依赖库存放在此目录 */
├── load.php           /** 插件载入文件 */
├── plugin.php         /** 插件入口文件，入口文件的文件名必须是插件的slug */
├── route.php          /** 路由表，由文派开发框架读取并向WordPress核心注册 */
├── config.php         /** 项目配置文件，由文派开发框架读取并提供调用方法 */
├── composer.json      /** Composer配置文件 */
├── composer.lock      /** Composer依赖关系锁定文件 */
├── .gitignore         /** Git排除项 */
├── license.txt        /** 版权声明文件 */
└── readme.md          /** 项目自述文件 */
```

### 插件入口文件约定

入口文件主要职责是标识插件元信息以及引入插件载入文件。以下是一个示例：

```php 
<?php
/**
 * Plugin Name: plugin
 * Description: this is a plugin.
 * Author: WenPai.org
 * Author URI:https://wenpai.org/
 * Text Domain: plugin
 * Domain Path: /languages
 * Version: 1.0.0
 * Network: True
 * License: GPLv2 or later
 * License URI: http://www.gnu.org/licenses/gpl-2.0.html
 */
 
namespace WenPai\Plugin
 
/** 装载插件 */
require_once( 'load.php' );
```

### 插件装载文件约定

插件装载文件负责加载框架、Composer，初始化插件核心、装载功能模块。以下是一个示例：

```php
<?php
/**
 * 插件装载文件
 *
 * @package WenPai\Plugin
 */

use WenPai\Framework\Init;
use WenPai\Plugin\Src\{ Register_Widget, Register_Setting };

/** 载入Composer的自动装载机制 */
require_once( 'vendor/autoload.php' );

/** 载入文派开发框架 */
new Init();

/** 注册小工具 */
new Register_Widget();

/** 注册设置页 */
new Register_Setting();

/** 
 * 载入插件核心 
 * 
 * 请保持在初始化时为类传入设置的习惯，在类内部应只判断成员变量而非设置项。
 */
$core = new Core( array(
    'setting1' => get_option( 'setting1', '1' ),
    'setting2' => get_option( 'setting2', '2' ),
    'setting3' => get_option( 'setting3', '3' ),
) );

/** 
 * 注册插件核心的钩子 
 * 
 * 每个类都应该有一个专门的函数负责注册该类所挂载的所有钩子，不要让钩子注册分散的到处都是。
 */
$core->register_hook();
```

### 功能模块约定

编程是对问题和事物的建模，必须对问题or事物拆解、归类，最终总结出清晰的结构、模块划分才能顺利的使用编程语言建模。

每个功能模块在源代码中对应一个类，模块可以拥有父模块和子模块。

绝对要避免全局变量、函数满天飞的情况，出现这种情况不仅仅意味着未来可能的命名冲突，更意味着这个程序没经过认真的建模、没有清晰的结构，仅仅是Ctrl c+v拼凑出来的。未来的可维护性和可扩展性都需要打一个问号。

具体功能模块的编写可以自由发挥，但需要遵守一些基本约定：

 1. 模块中不要直接读取插件设置项，设置项的值应该通过在模块初始化时传递给构造函数，并由构造函数处理（设置默认值、数据消毒等）后传递给成员变量。
 2. 模块的钩子应该统一在名为register_hook的方法中注册，不要让钩子注册分散到各处。该register_hook方法应该由模块调用者显性调用，而不应该由模块的构造函数调用。

以下是示例：

```php 
<?php
/**
 * 功能模块演示类
 *
 * @package WenPai\Plugin
 */

namespace WenPai\Src;

/**
 * 核心功能类
 *
 * @since 1.0.0
 */
class Core {

    /** 
     * Test1
     * 
     * @since 1.0.0
     * @var string
     */
    private $test1;
    
    /** 
     * Test2
     * 
     * @since 1.0.0
     * @var string
     */
    private $test2;
    
    /** 
     * Test3
     * 
     * @since 1.0.0
     * @var string
     */
    private $test3;
    
    /**
     * 构造函数
     *
     * @since 1.0.0
     * @param array $args {
     *     @type string $test1 Test1
     *     @type string $test2 Test2
     *     @type string $test3 Test3
     * }
     */
    public function __construct( array $args ) {
        $defaults = array(
            'test1' => '1',
            'test2' => '2',
            'test3' => '3',
        );
        $args = wp_parse_args( $args, $defaults );
    
        $this->test1 = sanitize_text( $args['test1'] );
        $this->test2 = sanitize_text( $args['test2'] );
        $this->test3 = sanitize_text( $args['test3'] );
    }

    /**
     * 注册核心功能的钩子
     *
     * @since 1.0.0
     */
    public function register_hook() {
        add_action( 'init', array($this, 'test') );
    }

    /**
     * 打印所有的成员变量
     *
     * @since 1.0.0
     */
    public function test() {
        echo $this->test1;
        echo $this->test2;
        echo $this->test3;
    }
    
}
```

## 主题结构规范

待编写