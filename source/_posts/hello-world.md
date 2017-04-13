---
title: MyBlog
---
Welcome to [west13](https://westfive.github.io/)! 
以下：一些关于搭建Blog的文章

### 如何搭建一个独立博客——简明Github Page与Hexo教程
``` bash
概要：
$git checkout --orphan gh-pages
在Git中，分支(branch)的概念非常重要，Git之所以强大，很大程度上就是因为它强大的分支体系。这里的分支名字必须是gh-pages，
因为github规定，在项目类型的仓库中，只有该分支中的页面，才会生成网页文件。
# 将当前的改动暂存在本地仓库
$ git add .
# 将暂存的改动提交到本地仓库，并写入本次提交的注释是”first post“
$ git commit -m "first post"
# 将远程仓库在本地添加一个引用：origin
$ git remote add origin https://github.com/username/projectName.git
# 向origin推送gh-pages分支，该命令将会将本地分支gh-pages推送到github的远程仓库，并在远程仓库创建一个同名的分支。该命令后会提示输入用户名和密码。
$ git push origin gh-pages
```
更多信息: [点我](http://www.jianshu.com/p/05289a4bc8b2)


### 手把手教你搭建一个博客

``` bash
$ 摘要：
$ hexo n == hexo new
$ hexo g == hexo generate
$ hexo s == hexo server
$ hexo d == hexo deploy
1 $ hexo d -g #生成部署
2 $ hexo s -g #生成预览
——————————————————————————————
clone github repo
$ cd d:/hexo/blog
$ git clone https://github.com/jiji262/jiji262.github.io.git .deploy/jiji262.github.io
将我们之前创建的repo克隆到本地，新建一个目录叫做.deploy用于存放克隆的代码。
创建一个deploy脚本文件
hexo generate
cp -R public/* .deploy/jiji262.github.io
cd .deploy/jiji262.github.io
git add .
git commit -m “update”
git push origin master
简单解释一下，hexo generate生成public文件夹下的新内容，
然后将其拷贝至jiji262.github.io的git目录下，
然后使用git commit命令提交代码到jiji262.github.io这个repo的master branch上。
需要部署的时候，执行这段脚本就可以了
（比如可以将其保存为deploy.sh）。
执行过程中可能需要让你输入Github账户的用户名及密码，按照提示操作即可。

```

更多信息: [点我](http://jiji262.github.io/2016/04/15/2016-04-15-hexo-github-pages-blog/)

### 献给写作者的 Markdown 新手指南

``` bash
在此，我们总结 Markdown 的优点如下：

纯文本，所以兼容性极强，可以用所有文本编辑器打开。
让你专注于文字而不是排版。
格式转换方便，Markdown 的文本你可以轻松转换为 html、电子书等。
Markdown 的标记语法有极好的可读性。
当然，我们既然如此推崇 Markdown ，也必定会教会你使用 Markdown ，这也是本文的目的所在。不过，虽然 Markdown 的语法已经足够简单，但是现有的 Markdown 语法说明更多的是写给 web 从业者看的，对于很多写作者来说，学习起来效率很低，现在，我们特地为写作者量身定做本指南，从写作者的实际需求出发，介绍写作者真正实用的常用格式，深入浅出、图文并茂地让您迅速掌握 Markdown 语法。

文／简书（简书作者）
原文链接：http://www.jianshu.com/p/q81RER
著作权归作者所有，转载请联系作者获得授权，并标注“简书作者”。
```

更多信息: [点我](http://www.jianshu.com/p/q81RER)

**————————————————————————————————我是有底线的———————————————————————————————————————**

个人微博: [西十三lotos](http://weibo.com/u/6076206582?is_hot=1)
