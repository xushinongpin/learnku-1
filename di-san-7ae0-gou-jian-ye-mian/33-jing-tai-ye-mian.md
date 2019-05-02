## 说明

在进行完项目的基本配置之后，我们要开始正式进行项目开发了。本节将通过三个基本静态页面的构建，来让你了解 Laravel 最基本的页面构建流程。本书在接下来都会为每个章节新建一个 Git 分支，并在该分支上进行功能开发，开发完毕之后再将新建的分支合并到主分支上。

## 新建分支

首先让我们使用 Git 来新建一个 static-pages 分支。

```
$ git checkout master
$ git checkout -b static-pages
```

上面的第一条命令`git checkout master`代表将当前分支切换到 master 分支上，master 分支是我们初始化 Git 时默认创建的主分支，其它分支都是基于主分支衍生出来的。

第二条命令`git checkout -b static-pages`将会为你创建一个名为`static-pages`的新分支。`-b`选项表示创建指定名称的新分支。

你可以将新建的`static-pages`理解为是对 master 分支的克隆，在上面做的所有修改都不会影响到 master 分支。本节后面会将`static-pages`分支合并到 master 分支上，合并成功之后，在`static-pages`分支上做的所有改动都会并入到 master 分支。另外，你也可以选择对一个分支进行删除操作，当一个分支被删除之后，在该分支之上的所有改动也都将被销毁，删除分支的操作不会影响到 master 分支。这便是 Git 工作流的强大之处。

合并分支示例：

```
$ git merge fake-branch
```

删除分支示例：

```
$ git branch -d fake-branch
```

## 移除无用视图

Laravel 默认会为我们生成一个`welcome.blade.php`视图文件，主要用于对默认页面进行渲染，这个文件对我们接下来的项目开发没有一点用处，因此我们可将其移除：

```
$ rm resources/views/welcome.blade.php
```

## 配置路由

当用户在查看一个网页时，一个完整的访问过程如下：

1. 打开浏览器在地址栏输入 URL 并访问；
2. 路由将 URL 请求映射到指定控制器上；
3. 控制器收到请求，开始进行处理。如果视图需要动态数据进行渲染，则控制器会开始从模型中读取数据；
4. 数据读取完毕，将数据传送给视图进行渲染；
5. 视图渲染完成，在浏览器上呈现出完整页面；

如下图：

[![](https://iocaffcdn.phphub.org/uploads/images/201705/13/1/VptHggpp0v.png "file")](https://iocaffcdn.phphub.org/uploads/images/201705/13/1/VptHggpp0v.png)

在 Laravel 开发中，我们使用路由来定义 URL 和 URL 的请求方式，再将该 URL 分配到相对应的控制器动作中进行处理。接下来要构建三个静态页面分别是主页、帮助页、关于页。因此我们需要为路由指定好三个不同的 URL：

_routes/web.php_

```
<?php

Route::get('/', 'StaticPagesController@home');
Route::get('/help', 'StaticPagesController@help');
Route::get('/about', 'StaticPagesController@about');
```

在上面代码的代码中，我们为 get 方法传递了两个参数，第一个参数指明了 URL，第二个参数指明了处理该 URL 的控制器动作。get 表明这个路由将会响应 GET 请求，并将请求映射到指定的控制器动作上。比方说，我们向[http://weibo.test/](http://weibo.test/)发出了一个请求，则该请求将会由`StaticPagesController`的`home`方法进行处理。我们将在下节创建`StaticPagesController`，为你讲解控制器在收到请求后如何进行相关操作。

在 Laravel 中我们较为常用的几个基本的 HTTP 操作分别为 GET、POST、PATCH、DELETE。

* GET 常用于页面读取
* POST 常用于数据提交
* PATCH 常用于数据更新
* DELETE 常用于数据删除

在这四个动作中，PATCH 和 DELETE 是不被浏览器所支持的，但我们可以通过在提交表单中做一些手脚，让服务器以为这两个动作是从浏览器中发出的一样，后面我会具体讲解如何在表单中通过添加隐藏域的方式来欺骗服务器。这里你只需要有个大概的印象即可。

## 生成静态页面控制器

要让静态页面在网站上进行展示，我们需要先创建一个`StaticPagesController`控制器，这个控制器将负责整个网站静态页面的处理。Laravel 的控制器命名规范统一使用[驼峰式大小写](http://baike.baidu.com/view/2359058.htm)和复数形式来命名，在这里我们也应该这么做。一般情况下，我们会使用下面命令来生成静态页面控制器：

```
$ php artisan make:controller StaticPagesController
```

让我们来看下`StaticPagesController`文件生成的默认代码：

_app/Http/Controllers/StaticPagesController.php_

```

<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class StaticPagesController extends Controller
{
    //
}
```

`namespace`代表的是[命名空间](http://baike.baidu.com/view/94233.htm)，这是在 PHP 5.3 之后才加入的语言特性，如果你之前学习过 Java 或 C\# 的话，那么你应该对命名空间不陌生。简单来说，开发者可以利用命名空间来区分归类不同的代码功能，避免引起变量名或函数名的冲突。你可以把命名空间理解为文件路径，把变量名理解为文件。当我们在不同路径分别存放了相同的文件时，系统就不会出现冲突。

我们用`use`来引用在 PHP 文件中要使用的类，引用之后便可以对其进行调用。

最后我们看到，在静态页面控制器中还定义了一个`StaticPagesController`类，这个类继承了父类`App\Http\Controllers\Controller`，这意味着你可以在`StaticPagesController`类中任意使用父类中除私密方法外的其它方法。

现在的静态页面控制器中还没有指定好三个页面对应的动作，让我们来为控制器加上这三个动作来处理从路由发过来的请求：

_app/Http/Controllers/StaticPagesController.php_

```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class StaticPagesController extends Controller
{
    public function home()
    {
        return '主页';
    }

    public function help()
    {
        return '帮助页';
    }

    public function about()
    {
        return '关于页';
    }
}
```

在`StaticPagesController`类中，我们新定义了三个方法，这三个方法在接受到路由发过来的请求时，将会返回各自页面的名称，这三个方法名称与路由上的定义一一对应。现在：

* 访问
  [http://weibo.test/](http://weibo.test/)
  你将看到有 "主页" 二字输出
* 访问
  [http://weibo.test/help](http://weibo.test/help)
  页面，则会看到 "帮助页"
* 访问
  [http://weibo.test/about](http://weibo.test/about)
  页面，则会看到 "关于页"

这三个方法返回的只是纯文本内容，算不上真正的视图，接下来我们再来添加并渲染真正的视图。

## 添加静态页面视图

要在控制器中指定渲染某个视图，则需要使用到`view`方法，`view`方法接收两个参数，第一个参数是视图的路径名称，第二个参数是与视图绑定的数据，第二个参数为可选参数。本章节讲的是静态页面的构建，因此并不需要利用到第二个参数，在后面的教程中我们会再讲解如何为视图绑定数据，并在视图中使用绑定的数据。

`view`方法在控制器的应用如下：

_app/Http/Controllers/StaticPagesController.php_

```
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;

class StaticPagesController extends Controller
{
    public function home()
    {
        return view('static_pages/home');
    }

    public function help()
    {
        return view('static_pages/help');
    }

    public function about()
    {
        return view('static_pages/about');
    }
}
```

下面这行代码，将会渲染在`resources/views`文件夹下的`static_pages/home.blade.php`文件。默认情况下，所有的视图文件都存放在`resources/views`文件夹下。

```
return view('static_pages/home');
```

在控制器中指定渲染的视图之后，接下来便是对视图进行构建了，我们需要在`resources/views`中新增下面三个视图。

_resources/views/static\_pages/home.blade.php_

```
<!DOCTYPE html>
<html>
<head>
  <title>Weibo App</title>
</head>
<body>
  <h1>主页</h1>
</body>
</html>
```

_resources/views/static\_pages/help.blade.php_

```
<!DOCTYPE html>
<html>
<head>
  <title>Weibo App</title>
</head>
<body>
  <h1>帮助页</h1>
</body>
</html>
```

_resources/views/static\_pages/about.blade.php_

```
<!DOCTYPE html>
<html>
<head>
  <title>Weibo App</title>
</head>
<body>
  <h1>关于页</h1>
</body>
</html>
```

## Blade 模板

细心的你可能会留意到这三个文件的后缀名均为`.blade.php`，而不是`.php`。这是因为 Blade 是 Laravel 中提供的一套模板引擎，在 Blade 视图中我们可以使用 Laravel 为这套引擎定义的一些默认方法，并完全兼容 PHP 语法的书写。在项目运行时，Laravel 会把所有的 Blade 视图进行编译缓存成普通的 PHP 代码，因此你不必担心 Blade 会对应用产生负担。

现在所有视图已经创建完毕，访问对应的 URL 地址便能看到视图被成功渲染。

[![](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/ZlEguyCjBr.png!large "file")](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/ZlEguyCjBr.png!large)

[![](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/f2EOaghMNR.png!large "file")](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/f2EOaghMNR.png!large)

[![](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/jZBPzw7RhA.png!large "file")](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/jZBPzw7RhA.png!large)

## 使用通用视图

你可能已经注意到了，前面我们创建的几个视图里面包含着一些重复的代码，这明显违反了 DRY（Don't repeat yourself）原则，导致代码变得不够灵活、简洁。因此我们需要对页面进行重构，把多余的代码从视图中抽离出来，单独创建一个默认视图来进行存放通用代码。

我们给应用创建了一个 default 视图，并将其放在`layouts`文件夹中，default 视图将作为整个应用的基础视图。实际上你只要保证视图文件被放置在`resources/views`目录下即可，Laravel 对视图的文件夹和文件命名并没有限制，我将 default 文件放在`layouts`文件下，只是为了让应用的目录结构让人更好理解。

_resources/views/layouts/default.blade.php_

```
<!DOCTYPE html>
<html>
  <head>
    <title>Weibo App</title>
  </head>
  <body>
    @yield('content')
  </body>
</html>
```

下面的这行代码表示该占位区域将用于显示`content`区块的内容，而`content`区块的内容将由继承自 default 视图的子视图定义。

```
@yield('content')
```

Laravel 的 Blade 模板支持继承，这意味多个子视图可以共用父视图提供的视图模板。接下来让我们修改之前创建的首页视图文件，来学习下如何使用 Blade 模板的继承。

_resources/views/static\_pages/home.blade.php_

```
@extends('layouts.default')
@section('content')
  <h1>主页</h1>
@stop
```

我们使用了`@extends`并通过传参来继承父视图`layouts/default.blade.php`的视图模板。

```
@extends('layouts.default')

```

使用`@section`和`@stop`代码来填充父视图的`content`区块，所有包含在`@section`和`@stop`中的代码都将被插入到父视图的`content`区块。

```
@section('content')
  <h1>主页</h1>
@stop
```

在 Laravel 对`home.blade.php`文件进行编译后，结果如下：

```
<!DOCTYPE html>
<html>
  <head>
    <title>Weibo App</title>
  </head>
  <body>
    <h1>主页</h1>
  </body>
</html>
```

由此可见 Blade 模板继承有多方便。接下来让我们对其它两个视图也进行更改，统一使用父视图的代码。

_resources/views/static\_pages/help.blade.php_

```
@extends('layouts.default')
@section('content')
  <h1>帮助页</h1>
@stop
```

_resources/views/static\_pages/about.blade.php_

```
@extends('layouts.default')
@section('content')
  <h1>关于页</h1>
@stop
```

修改完成之后，再次在网页上访问这几个静态页面，你会发现父视图的代码已被成功嵌入到子视图中。

现在还有一点不足的地方，就是所有的网站标题名字都为 Weibo，这可太没个性了。因此我们接下来要做的就是针对页面标题进行优化，让不同页面显示不同的标题。

首先我们要修改默认视图文件，在代码显示标题的位置嵌入`@yield`区块：

_resources/views/layouts/default.blade.php_

```
<!DOCTYPE html>
<html>
  <head>
    <title>@yield('title', 'Weibo App')</title>
  </head>
  <body>
    @yield('content')
  </body>
</html>
```

我们给`@yield`传了两个参数，第一个参数是该区块的变量名称，第二个参数是默认值，表示当指定变量的值为空值时，使用`Weibo`来作为默认值。

```
<title>@yield('title', 'Weibo App')</title>
```

下面让我们来为帮助页和关于页加上指定标题。

_resources/views/static\_pages/help.blade.php_

```
@extends('layouts.default')
@section('title', '帮助')

@section('content')
  <h1>帮助页</h1>
@stop
```

_resources/views/static\_pages/about.blade.php_

```
@extends('layouts.default')
@section('title', '关于')

@section('content')
  <h1>关于页</h1>
@stop
```

注意的是，当`@section`传递了第二个参数时，便不需要再通过`@stop`标识来告诉 Laravel 填充区块会在具体哪个位置结束。

我们也可以在`@yield`区块后面进行内容拼接。让我们标题拥有更加丰富的信息。



_resources/views/layouts/default.blade.php_

```
<!DOCTYPE html>
<html>
  <head>
    <title>@yield('title', 'Weibo App') - Laravel 新手入门教程</title>
  </head>
  <body>
    @yield('content')
  </body>
</html>
```

现在到浏览器重新刷新下页面，应该能看到标题后面成功拼接上我们定义的内容了。

[![](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/lVecQ2V0al.png!large "file")](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/lVecQ2V0al.png!large)

## Git 代码版本控制

接着让我们将本次更改纳入版本控制中：

```
$ git add -A
$ git commit -m "基础页面"
```



