---
title: npm常遇到的几个坑
---
![](http://ohe5u4k9s.bkt.clouddn.com/u=4151515598,4093873234&fm=21&gp=0.jpg)

**npm安装**

NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题，常见的使用场景有以下几种：
允许用户从NPM服务器下载别人编写的第三方包到本地使用。 
允许用户从NPM服务器下载并安装别人编写的命令行程序到本地使用。 
允许用户将自己编写的包或命令行程序上传到NPM服务器供别人使用。
由于新版的nodejs已经集成了npm，所以之前npm也一并安装好了。同样可以通过输入 "npm -v" 来测试是否成功安装。命令如下，出现版本提示表示安装成功: 

```
$ npm -v
2.3.0
```

如果你安装的是旧版本的 npm，可以很容易得通过 npm 命令来升级，命令如下：
```
$ sudo npm install npm -g
/usr/local/bin/npm -> /usr/local/lib/node_modules/npm/bin/npm-cli.js
npm@2.14.2 /usr/local/lib/node_modules/npm
```
如果是 Window 系统使用以下命令即可：
```
npm install npm -g
```


>这里有一个大坑
我用cnpm install -g npm升级了npm之后，使用npm -v显示版本为2.11.2
然而我安装yoeman的时候，它提示我的npm版本过低，是2.7.6，请问这是怎么回事呢？

解决方案：
```
复制AppData\Roaming\npm\node_modules\npm下的文件到你的NodeJS安装目录下的 \node_modules\npm 中，覆盖掉原来的全部文件。
```

**使用cnpm和snpm**
>为什么要使用CNPM#
在我们中国，要下载 npm 包非常慢，如果使用 cnpm 下载包就非常快了，感觉很爽，但是 cnpm 也有几个大问题：

1.cnpm 的仓库只是 npm 仓库的一个拷贝，它不承担 publish 工作，所以你用 cnpm publish 命令会执行失败的

2.不仅是 publish 会执行失败，其它的需要注册用户(npm adduser)、或者修改 package 状态等命令都无法用 cnpm

3.有很多 npm 包都集成了 npm install，比如 yeoman 的所有 generators ，在最后基本都会调用 npm install，（ 可以看其源码 ）这种情况下使用 cnpm 完全无效，必须中断操作，然后自己手动运行 cnpm install，或者在运行 yo [generator] 时就指定 --skip-install，这体验就很不爽了

4.还有一种情况是，很多和 npm API 相关的 package，都会读取 ~/.npmrc 中的 registry，或者使用默认的 registry —— [https://registry.npmjs.org/][npm-registry]，去查询 npm package 相关的信息，比如下面这些：

npm-latest: 查询某个 package 的最新版本号
npm-name: 查询某个 package name 是否被注册了
npm-dependents: 查找某个模块所依赖的其它所有模块
…
如果你用的任何一个包或其所依赖的包中用了这些 package，那么在这些包请求网络时也得慢死了！
>来使用snpm吧
[SnpmGithub](https://github.com/WestFive/smart-npm)
**安装**
```
npm install --global smart-npm --registry=https://registry.npm.taobao.org/
```
安装成功后默认会在你的 npm 用户配置文件 ~/.npmrc 中添加淘宝的 registry。
**替换原始npm命令**
```
Window 用户需要先定位到 npm.cmd 和 smart-npm.cmd 两个文件，然后用 smart-npm.cmd 替换 npm.cmd（注意备份原来的 npm.cmd），
同时注意修改下 smart-npm.cmd 文件里的路径，否则运行 npm 会报找不到文件错误（如果不明白，建议不要替换，直接使用 snpm 或 smart-npm）
可以使用命令 where smart-npm 来定位到 smart-npm.cmd 文件所在的位置。如：在我的系统上执行 where smart-npm 的结果是：

C:\Users\Mora>where smart-npm
C:\Program Files\nodejs\smart-npm
C:\Program Files\nodejs\smart-npm.cmd
```

Just Use It ..
如果有疑问，请[SnpmGithub](https://github.com/WestFive/smart-npm)



**————————————————————————————————我是有底线的———————————————————————————————————————**

个人微博: [西十三lotos](http://weibo.com/u/6076206582?is_hot=1)
