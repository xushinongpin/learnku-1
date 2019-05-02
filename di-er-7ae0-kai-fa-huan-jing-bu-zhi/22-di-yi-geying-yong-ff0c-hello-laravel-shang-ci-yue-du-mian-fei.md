## 第一个应用，Hello Laravel！

这一节我们来创建一个基本的 Hello World Laravel 应用。上一节已经把环境部署好了，在上节的最后我们关闭了虚拟机。此时我们得先开启 Homestead 虚拟机：

```
> cd ~/Homestead && vagrant up
> vagrant ssh
```

虚拟机启动成功后，通过下面命令来新建一个名为 Laravel 的项目：

```
$ cd ~/Code
$ composer create-project laravel/laravel Laravel --prefer-dist "5.8.*"
```

完成之后，访问[http://homestead.test](http://homestead.test/)你能看到如下图所示界面，这是 Laravel 为我们生成默认界面。

[![](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/0hSv5Sld9h.png!large "file")](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/0hSv5Sld9h.png!large)

> 注：建议使用`composer create-project`命令来进行创建 Laravel 项目，这样能利用 Composer 的本地缓存功能和[Composer 中文镜像](https://learnku.com/laravel/t/4484/composer-mirror-use-help)来达到下载速度最优，尤其是后续的项目创建。

打开 Code 文件夹可以看到刚刚创建的 Laravel 应用，Laravel 默认会为我们生成了一堆文件和文件夹，每一个文件的置放目录和位置都有它的用意。这些目录结构都是经过 Laravel 作者精心设计的，为的就是统一开发规范，强调约定高于配置的原则。

[![](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/Sg19TSlByH.png!large "file")](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/Sg19TSlByH.png!large)

## 文件夹结构简介

表 1.1：Laravel 文件夹结构简介

| 文件夹名称 | 简介 |
| :--- | :--- |
| app | 应用程序的业务逻辑代码存放文件夹 |
| app/Console | 存放自定义 Artisan 命令文件 |
| app/Http/Controllers | 存放控制器文件 |
| app/Http/Middleware | 存放「中间件」文件 |
| bootstrap | 框架启动与自动加载设置相关的文件 |
| composer.json | 应用依赖的扩展包 |
| composer.lock | 扩展包列表，确保这个应用的副本使用相同版本的扩展包 |
| config | 应用程序的配置文件 |
| database | 数据库操作相关文件（数据库迁移和数据填充） |
| node\_modules | 存放 NPM 依赖模块 |
| package.json | 应用所需的 NPM 包配置文件 |
| phpunit.xml | 测试工具 PHPUnit 的配置文件 |
| public | 前端控制器和资源相关文件（图片、JavaScript、CSS） |
| readme.md | 项目介绍说明文件 |
| resources | 应用资源 |
| resources/js | 未编译的 JavaScript 代码 |
| resources/sass | 未编译的 SASS 代码 （将会编译为 CSS ） |
| resources/lang | 多语言文件 |
| resources/views | 视图文件 |
| routes/api.php | 用于定义 API 类型的路由 |
| routes/channels.php | 事件转播注册信息 |
| routes/console.php | 用于定义 Artisan 命令 |
| routes/web.php | 用于定义 Web 类型的路由（重点，大部分情况下本书会用到） |
| server.php | 使用 PHP 内置服务器时的 URL 重写（类似于 Apache 的 "mod\_rewrite" ） |
| storage | 编译后的视图、基于会话、文件缓存和其它框架生成的文件 |
| storage/app | 目录可用于存储应用程序使用的任何文件 |
| storage/framework | 目录被用于保存框架生成的文件及缓存 |
| storage/logs | 应用程序的日志文件 |
| tests | 应用测试相关文件 |
| vendor | Composer 依赖模块 |
| webpack.mix.js | Laravel 的前端工作流配置文件 |
| yarn.lock | Yarn 依赖版本锁定文件 |
| .gitignore | 被 Git 所忽略的文件 |
| .env | 环境变量配置文件 |

## Composer

[Composer](https://getcomposer.org/)是一款跨平台的 PHP 依赖管理工具，其创作灵感来源于 Node.js 的 NPM 与 Ruby 的 Bundler。Laravel 使用 Composer 来作为扩展包的管理工具。你可以利用 Composer 结合[其它开源扩展包](https://packagist.org/)来达到快速建站的目的。打开`composer.json`文件，可以看到 Laravel 默认集成了一些为框架提供支持的扩展包。

Composer 是一个伟大的发明，他让组件式编程成为可能，编写软件时，就如拼接乐高玩具一样。极大的提高了开发的效率和代码的可复用性，解放了生产力。

_composer.json_

```
{
    "name": "laravel/laravel",
    "type": "project",
    "description": "The Laravel Framework.",
    "keywords": [
        "framework",
        "laravel"
    ],
    "license": "MIT",
    "require": {
        "php": "^7.1.3",
        "fideloper/proxy": "^4.0",
        "laravel/framework": "5.8.*",
        "laravel/tinker": "^1.0"
    },
    "require-dev": {
        "beyondcode/laravel-dump-server": "^1.0",
        "filp/whoops": "^2.0",
        "fzaninotto/faker": "^1.4",
        "mockery/mockery": "^1.0",
        "nunomaduro/collision": "^2.0",
        "phpunit/phpunit": "^7.5"
    },
    "config": {
        "optimize-autoloader": true,
        "preferred-install": "dist",
        "sort-packages": true
    },
    "extra": {
        "laravel": {
            "dont-discover": []
        }
    },
    "autoload": {
        "psr-4": {
            "App\\": "app/"
        },
        "classmap": [
            "database/seeds",
            "database/factories"
        ]
    },
    "autoload-dev": {
        "psr-4": {
            "Tests\\": "tests/"
        }
    },
    "minimum-stability": "dev",
    "prefer-stable": true,
    "scripts": {
        "post-autoload-dump": [
            "Illuminate\\Foundation\\ComposerScripts::postAutoloadDump",
            "@php artisan package:discover --ansi"
        ],
        "post-root-package-install": [
            "@php -r \"file_exists('.env') || copy('.env.example', '.env');\""
        ],
        "post-create-project-cmd": [
            "@php artisan key:generate --ansi"
        ]
    }
}
```

该文件使用 JSON 格式编写，`require`键对应的是应用在 Laravel 所有环境上的扩展包，`require-dev`键对应的是应用在 Laravel 开发环境上的扩展包。

在添加扩展包到`composer.json`时，需要为扩展包指定版本号才能进行安装。我们从`composer.json`文件中可以看到有以下这三种方式来为扩展包来指定版本范围。

第一种如下所示：

```
"php": "^7.1.3",
```

这行代码表示安装版本号大于或等于 7.1.3 版本的 PHP。

第二种是：

```
"laravel/framework": "5.8.*",
```

这行代码表示安装在 5.8.0 以上，5.9.0 以下的最新 Laravel 框架，它有可能是`5.8.0`甚至是`5.8.9`。

> 新手的话不要求完全弄懂 Composer，上面的知识让你能跟着教程继续下去，后面等掌握此书的知识了，慢慢能站稳脚跟了，再继续深入学习。

## 第一行 Laravel 代码

现在让我们来写下第一行 Laravel 代码，在页面上加入一些带有我们个人身份信息的内容。

Laravel 在项目创建时会自动为我们生成一个`welcome.blade.php`文件，这个文件将被用于渲染 Laravel 的默认视图。现在，让我们打开该文件，复制替换为以下内容，并加入你自己的一些个人信息。

_resources/views/welcome.blade.php_

```
<!DOCTYPE html>
<html>
    <head>
        <title>Laravel</title>
        <style>
            html, body {
                height: 100%;
            }

            body {
                margin: 0;
                padding: 0;
                width: 100%;
                display: table;
                font-weight: 100;
                font-family: "Helvetica Neue", NotoSansHans-Regular,AvenirNext-Regular,arial,Hiragino Sans GB,"Microsoft Yahei","Hiragino Sans GB","WenQuanYi Micro Hei",sans-serif;
                color:#777;
            }

            .container {
                text-align: center;
                display: table-cell;
                vertical-align: middle;
            }

            .content {
                text-align: center;
                display: inline-block;
            }

            .title {
                font-size: 66px;
            }
        </style>
    </head>
    <body>
        <div class="container">
            <div class="content">
                <div class="title">Hello Laravel! - by Summer</div>
            </div>
        </div>
    </body>
</html>
```

上面的 Summer 是我的常用网络 ID，你也可以替换为自己的个人信息。

接着让我们重新打开[http://homestead.test](http://homestead.test/)页面，可看到我们的个人信息已经成功显示在页面上。

[![](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/NghpJP2hyM.png!large "file")](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/NghpJP2hyM.png!large)

