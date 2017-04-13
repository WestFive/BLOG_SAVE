---
title: WebPack学习笔记（三）
---
![](http://ohe5u4k9s.bkt.clouddn.com/what-is-webpack.png)
**配置文件**

Webpack 在执行的时候，除了在命令行传入参数，还可以通过指定的配置文件来执行。
默认情况下，会搜索当前目录的 webpack.config.js 文件，
这个文件是一个 node.js 模块，返回一个 json 格式的配置信息对象，
或者通过 --config 选项来指定配置文件。
继续我们的案例，在根目录创建 package.json 来添加 webpack 需要的依赖：
```
{
  "name": "webpack-example",
  "version": "1.0.0",
  "description": "A simple webpack example.",
  "main": "bundle.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [
    "webpack"
  ],
  "author": "zhaoda",
  "license": "MIT",
  "devDependencies": {
    "css-loader": "^0.21.0",
    "style-loader": "^0.13.0",
    "webpack": "^1.12.2"
  }
}
```

# 如果没有写入权限，请尝试如下代码更改权限
```
chflags -R nouchg .
sudo chmod  775 package.json
别忘了运行 npm install。
```

然后创建一个配置文件 webpack.config.js：
```
var webpack = require('webpack')

module.exports = {
  entry: './entry.js',
  output: {
    path: __dirname,
    filename: 'bundle.js'
  },
  module: {
    loaders: [
      {test: /\.css$/, loader: 'style!css'}
    ]
  }
}
```
同时简化 `entry.js `中的 `style.css` 加载方式：

```
require('./style.css')
```
最后运行 webpack，
可以看到 webpack 通过配置文件执行的结果和上一章节通过命令行
webpack entry.js bundle.js --module-bind 'css=style!css'
 执行的结果是一样的。
 
 **插件**

插件可以完成更多 loader 不能完成的功能。

插件的使用一般是在 webpack 的配置信息 plugins 选项中指定。

Webpack 本身内置了一些常用的插件，还可以通过 npm 安装第三方插件。

接下来，我们利用一个最简单的 BannerPlugin 内置插件来实践插件的配置和运行，这个插件的作用是给输出的文件头部添加注释信息。

修改 webpack.config.js，添加 plugins：
```
var webpack = require('webpack')

module.exports = {
  entry: './entry.js',
  output: {
    path: __dirname,
    filename: 'bundle.js'
  },
  module: {
    loaders: [
      {test: /\.css$/, loader: 'style!css'}
    ]
  },
  plugins: [
    new webpack.BannerPlugin('This file is created by zhaoda')
  ]
}
```
然后运行 webpack，打开 bundle.js，可以看到文件头部出现了我们指定的注释信息：
```
/*! This file is created by west13 */
/******/ (function(modules) { // webpackBootstrap
/******/  // The module cache
/******/  var installedModules = {};
// 后面代码省略
```

**————————————————————————————————我是有底线的———————————————————————————————————————**

个人微博: [西十三lotos](http://weibo.com/u/6076206582?is_hot=1)
