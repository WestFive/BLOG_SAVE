---
title: WebPack学习笔记（二）
---
![](http://ohe5u4k9s.bkt.clouddn.com/what-is-webpack.png)

**Loader**
Webpack本身只能处理JavaScript模块，如果需要处理其他类型的文件，
就需要使用loader进行转换。

Loader可以理解为模块和资源的转换器，它本身是一个函数，接受源文件作为参数
返回转换的结果。这样，我们就可已通过require来加载任何类型的模块或文件，比如
CoffeeScript.JSX.LESS或图片。

**Loader**特性：
*Loader可以通过管道方式链式调用，每个loader可以把资源转换成任意格式并传递给下一个loader,但是最后一个loader
*必须返回JavaScript.

*Loader可以同步或异步执行。

*Loader运行在node.js环境中，所以可以做任何可能的事情。

*Loader可以接受参数，以此来传递配置项给loader.

*Loader可以通过`npm`发布和安装。

*除了通过`package.json`和`main`指定，通常的模块也可以导出一个loader来使用。

*Loader可以访问配置。

*插件可以让loader拥有更多特性。

*Loader可以分发出附加的任意文件。

Loader本身也是运行在node.js环境中的JavaScript模块，
它通常会返回一个函数，大多数情况下
我们通过npm来管理loader,但是你也可以在项目中自己写loader模块.

按照惯例，而非必需，loader一般以 `xxx-loader`的方式命名，
`xxx`代表了这个loader要做的转换功能，比如说`json-loader`.

在引用loader的时候可以使用全名`json-loader`，
或者使用短名json.这个命名规则和搜索优先级顺序在webpack的
`resolveLoader.moduleTemplates`api 中定义。

```
Defaule:["*-webpack-loader","*-web-loader","*-loader","*"]
```
Loader 可以在 require() 引用模块的时候添加，也可以在 webpack 全局配置中进行绑定，
还可以通过命令行的方式使用。
接上一节的例子，我们要在页面中引入一个 CSS 文件 style.css，
首页将 style.css 也看成是一个模块，然后用 css-loader 来读取它，
再用 style-loader 把它插入到页面中。
```
/*style.css */
body {background: yellow;}
```
修改entry.js:
```
require("!style!css!./style.css")//载入CSS
document.write("It's works ")
document.write(require('./module.js'))
```

安装loader：
```
npm install css-loader style-loader
```
重新编译
```
webpack entry.js bundle.js 
```
刷新页面，可以看到黄色的页面背景了。
如果每次 require CSS 文件的时候都要写 loader 前缀，
是一件很繁琐的事情。我们可以根据模块类型（扩展名）来自动绑定需要的 loader。

将 entry.js 中的 require("!style!css!./style.css") 
修改为 require("./style.css") ，然后执行：
```
$ webpack entry.js bundle.js --module-bind 'css=style!css'

# 有些环境下可能需要使用双引号
$ webpack entry.js bundle.js --module-bind "css=style!css"

```
显然，这两种使用 loader 的方式，效果是一样的。





**————————————————————————————————我是有底线的———————————————————————————————————————**

个人微博: [西十三lotos](http://weibo.com/u/6076206582?is_hot=1)
