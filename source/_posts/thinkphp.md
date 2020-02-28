---
title: thinkphp5命令行操作
date: 2020-02-28 14:22:05
tags: 学习心得
categories: php
---

在引入tinkphp5的时候推荐使用Composer安装   执行

```
composer create-project topthink/think tp --prefer-dist
```

cd到tp5目录下  运行

```
php think
```

执行结果如下

```
Think Console version 0.1

Usage:
  command [options] [arguments]

Options:
  -h, --help            Display this help message
  -V, --version         Display this console version
  -q, --quiet           Do not output any message
      --ansi            Force ANSI output
      --no-ansi         Disable ANSI output
  -n, --no-interaction  Do not ask any interactive question
  -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

Available commands:
  build              Build Application Dirs
  clear              Clear runtime file
  help               Displays help for a command
  list               Lists commands
  session            Clear Seesion file
 make
  make:controller    Create a new resource controller class
  make:model         Create a new model class
 optimize
  optimize:autoload  Optimizes PSR0 and PSR4 packages to be loaded with classmaps too, good for production.
  optimize:config    Build config and common file cache.
  optimize:route     Build route cache.
  optimize:schema    Build database schema cache.
```

可以看到框架提供了很多操作命令  最基本的生成模块命令

先把根目录下的bulid.php打开  里面提供的生成模块的示例命令  

我们在application目录下面也创建一个build.php，文件内容如下

```php
<?php
return [
    // 生成应用公共文件
    '__file__' => ['common.php', 'config.php', 'database.php'],

    // 定义demo模块的自动生成 （按照实际定义的文件名生成）
    'demo'     => [
        '__file__'   => ['common.php'],
        '__dir__'    => ['behavior', 'controller', 'model', 'view'],
        'controller' => ['Index', 'Test', 'UserType'],
        'model'      => ['User', 'UserType'],
        'view'       => ['index/index'],
    ],
    // 其他更多的模块定义
];
```

然后运行 

```
php think bulid
```

可以看到在application目录下面demo模块已经自动完成

下面是生成控制器和模型文件

```
php think make:controller demo/Order  //生成控制器
php think make:model demo/Order  //生成模型
```

生成控制器的时候  默认是资源控制器 自带初始化方法  这里我们也可以创建一个空的控制器 运行

```
php think make:controller demo/Order --plain
```

下面讲一下如何扩展命令行的功能

在application下创建一个command目录  里面新增一个Session.php文件

然后里面的代码如下

```php
<?php
namespace app\command;
use think\console\Command;
use think\console\Input;
use think\console\input\Option;
use think\console\Output;
class Session extends Command
{
    protected function configure()
    {
        //   设置命令名字和额外选项和描述信息
        $this->setName('session')
        ->addOption('clear','d',Option::VALUE_NONE,'clear all session',null)
    ->setDescription('Clear Seesion file');
    }
    //   指令操作
    protected function execute(Input $input, Output $output)
    {
        //   获取指令选项名称
        $path = $input->getOption('clear');
        if($path){
          unset($_SESSION);
          $output->writeln("Clear Session Successed");
        }else{
          $output->writeln("Clear Nothing");
        }
    }
}
```

configure方法定义了命令的 名称，选项，功能描述

可以看出  这里的setName定义了这个指令的名字

addOption定义了这个指令的其他选项   其中的：

clear 是选项的名字 

d 对应是别名

Option对应选项的类型

 值对应没有输入值 CVALUE_NONE） 、输入值必须 CVALUE _REQUIRED）、输入值可选（VALUE_OPTIONAL）和数组输入值（VALUE IS ARRAY) 4 种。 

'clear all session'对应选项的的说明

null：默认值，没有的话就默认为null

execute 方法定义了具体功能

然后在application目录下面的command.php(没有的话需要创建) 添加下面的代码

```php
return [
  'app\command\Session',
];
```

然后运行php think 看一下是否注册成功 出现如下说明成功了

```
Available commands:
  build              Build Application Dirs
  clear              Clear runtime file
  help               Displays help for a command
  list               Lists commands
  session            Clear Seesion file
```

然后我们可以测试一下 运行

```
php think session --clear
//结果为
Clear Session Successed

//也可以使用别名
php think session --d
//结果为
Clear Session Successed

//如果输入指令无
php think session
//结果为
Clear Nothing

```

