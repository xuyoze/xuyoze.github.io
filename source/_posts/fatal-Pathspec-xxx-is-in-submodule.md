---
title: Git 错误 fatal Pathspec 'xxx' is in submodule
date: 2017-12-29 22:19:49
tags: git
---
![git](/images/git.png "post-cover")

下午在创建`git`仓库的时候,有一个文件夹出现了异常,导致上传到 github后只有一个空的文件夹.

而文件夹中无任何文件.

![error](/image/Snipaste_2017-12-29_22-26-11.png)

更换机器后,拉取仓储,重新添加文件后试图将文件添加到git报错.

```
fatal: Pathspec 'claudia' is in submodule...
```

查找网友解决方法,在此记录如下:

I have a repository on google code with my project.

I use Git source control.

It seems that when I try to add files to git from a specific directory, I get the following error:

fatal: Pathspec 'autoload_classmap.php' is in submodule 'module/CocktailMakerModule'
Now I'm not trying to add a submodule. I'm just trying to add a directory to git!

The result that I have now is that this directory is committed empty. So when I try to add specific files I get the above error message.

I checked and there isn't any other .git directory in that directory, so I'm really confused to why this has happened.

Best How To :
Still no idea how it happened. All documentation I read assumes I have a .git directory there but I don't.

I just did the following:

git rm -rf --cached CocktailMakerModule/
git add CocktailMakerModule/
That seems to resolve the issue.

**解决方法:**
1. 执行命令从本地仓储删除现有文件夹
```
git rm -rf --cached claudia/
```
2.添加文件后重新添加到本地仓储
```
git add claudia/
```

**参考**:
[fatal: Pathspec 'autoload_classmap.php' is in submodule 'module/CocktailMakerModule'](http://www.howtobuildsoftware.com/index.php/how-do/coYr/git-fatal-pathspec-autoload-classmapphp-is-in-submodule-module-cocktailmakermodule)
