## 小结

本章主要为大家介绍了 Laravel 对静态页面的基本构建过程，但现在的静态页面还是有点简陋，我们将在下一章为应用加入一些样式，让应用看起来更加美观。

## 提交代码

将 Git 切换到 master 分支，并合并 static-pages 分支上的修改：

```
$ git checkout master
$ git merge static-pages
```

最后将代码推送到 GitHub 和 Heroku 上：

```
$ git push                     # 推送到 Github 上
$ git push heroku master       # 上线到 Heorku
```

查看我们的线上应用：

[![](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/4eKleqayvo.png!large "file")](https://iocaffcdn.phphub.org/uploads/images/201812/12/1/4eKleqayvo.png!large)

## 我们学习到什么？

经过本章节的学习，我们学到了以下内容：

* 对新建的 Laravel 项目进行基本配置；
* 手动创建静态视图、控制器；
* 了解控制器与视图的基本协作流程；
* 了解如何使用通用视图；
* 了解 Artisan 命令的基本使用；



