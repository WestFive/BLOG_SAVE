---
title: webPack学习笔记（一）
---
![](http://ohe5u4k9s.bkt.clouddn.com/what-is-webpack.png)

**安装**

用npm 安装Webpack:
```
$npm install webpack -g
```

创建项目安装依赖
```
# 进入项目目录
# 确定已经有 package.json，没有就通过 npm init 创建
# 安装 webpack 依赖
$ npm install webpack --save-dev
```
Webpack 目前有两个主版本，
一个是在 master 主干的稳定版，
一个是在 webpack-2 分支的测试版，
测试版拥有一些实验性功能并且和稳定版不兼容，
在正式项目中应该使用稳定版。
```
#查看webpack版本信息
$ npm info webpack 
#安装指定版本的  webpack
$npm install webpack@1.12.x --save-dev
```

如果需要使用Webpack开发工具，需要单独安装：
```
$npm install webpack-dev-server --save-dev
```

**使用**
首先创建一个静态页面 index.html 和一个入口文件entry.js
```
<!-- index.html -->
<html>
<head>
  <meta charset="utf-8">
</head>
<body>
  <script src="bundle.js"></script>
</body>
</html>
```

```
// entry.js
document.write('It works.')
//document.write("<h2>西十三</h2>");
//document.write("<hr>")
//document.write("<img src='http://ohe5u4k9s.bkt.clouddn.com/CjYi96_UUAEgLgd.jpg'/>")
```

然后编译entry.js并打包到bundle.js:
```
$ webpack entry.js bundle.js
```

打包过程中会显示日志：
```
Hash: e964f90ec65eb2c29bb9
Version: webpack 1.12.2
Time: 54ms
    Asset     Size  Chunks             Chunk Names
bundle.js  1.42 kB       0  [emitted]  main
   [0] ./entry.js 27 bytes {0} [built]
```

接下来添加一个模块module.js并修改入口entry.js

```
//module.js
module.exports = 'Coooooooooooooooooooooooooooooooooool'
//module.exports='<img src="http://ohe5u4k9s.bkt.clouddn.com/CjYi96_UUAEgLgd.jpg"/>';
```

重新打包`webpack entry.js bundle.js`后刷新页面看到变化 ` It works cooooooooooooooooooooooooooooool`
```
Hash: 279c7601d5d08396e751
Version: webpack 1.12.2
Time: 63ms
    Asset     Size  Chunks             Chunk Names
bundle.js  1.57 kB       0  [emitted]  main
   [0] ./entry.js 66 bytes {0} [built]
   [1] ./module.js 43 bytes {0} [built]
```

**重点**
``
Webpack 会分析入口文件，解析包含依赖关系的各个文件。
这些文件（模块）都打包到 bundle.js 。W
ebpack 会给每个模块分配一个唯一的 id 并通过这个 id 索引和访问模块。
在页面启动时，`会先执行 entry.js 中的代码，`其它模块会在运行 require 的时候再执行。
``


**————————————————————————————————我是有底线的———————————————————————————————————————**

个人微博: [西十三lotos](http://weibo.com/u/6076206582?is_hot=1)
