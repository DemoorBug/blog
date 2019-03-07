---
title: webpack
date: 2019-01-06 20:29:16
tags: [webpack]
categories:
---



[webpack版本更迭](https://github.com/webpack/webpack/releases)
[社区投票](https://webpack.js.org/vote)
[官方文档](https://webpack.docschina.org/concepts/)
<!-- more -->
# 模块化开发

方爷,图虫

## js模块化
### 1. 命名空间
库名.类别名.方法名
```js
var NameSpace = {}
NameSpace.type = NameSpace.type || {}

NameSpace.type.method = function () {

}
```

### 2. COMMONJS 
诞生于node社区，只能在服务器端使用
一个文件为一个模块
通过module.exports 暴露模块接口
通过require就可以引入模块

因为commonjs运行在服务端，他的require是同步执行的
同步执行[http://wiki.commonjs.org/wiki/Modules/1.1.1]()


### 3. AMD/CMD/UMD
**AMD**
全称Async Module Definition
使用define定义模块
使用require加载模块
比较著名的库RequireJs
依赖前置，提前执行
```js
define(
  //模块名
  "alpha",
  //依赖
  ["require", "exports", "beta"],
  //模块输出
  function (require, exports, beta) {
    exports.verb = function () {
      return bata.verb();
      //Or:
      //Create missing node module:'beta'
      return require("beta").verb()
    }
  }
)

```

---
**CMD**
通用模块定义Common Module Definition
一个文件一个模块
使用define定义模块
使用require加载模块
产物SeaJs
尽可能懒执行

```js
// 所有模块都通过define定义
define(function (require, exports, module) {
  var $ = require('jquery');
  var Spinning = require('./spinning');

  //通过exports对外提供接口
  exports.doSomething = ...
  //或者通过module.exports 提供整个接口
  module.exports = ...
})
```

---
**UMD**
Universal Module Definition
通用解决方案
三个步骤
  判断是否支持AMD
  判断是否支持CommonJs
  如果都没有使用全局变量


### 4. es module
EcmaScript Module
一个文件一个模块
export / import
```js
import { name as myNamed, named } from 'src/mylib'
```
这样的写法和这个貌似是一样的
```js
import { name : myNamed, named } from 'src/mylib'
```
拿到全部方法，mylib可以使用他
```js
import * as mylib from 'src/mylib'
```
只是加载进来，没有任何引用
```js
import 'src/mylib'
```
---
**export, export default**
- export, export default 均可到处常量，函数，文件，模块等
- export 一个文件可以有多个，
- export default一个文件只能有一个
- 通过export方式导出，在导入时要加{},export default 则不用
- export 能直接导出变量表达式，export default则不行
```js
main.js
export const mapNict = 'map'
export const name = function () {
  console.log('name')
}
export default {
  name: function () {
    console.log('defalut')
  }
}
```
导入步骤
```js
app.js
import { mapNict, name } from './main.js'
import m from './main.js'

console.log(mapNict); //map
name() //name

m.name() //default

import * as myname from './main.js'

myname.default.name() //default

console.log(myname.mapNict) //map

myname.name() //name
```

### 5. Webpack支持
  - AMD(RequireJs) //只需要一些了解
  - ES Modules(推荐的) //主流
  - CommonJs //主流

---
## Css模块化
这点没看，以后有需要回来看吧，感觉没什么用

# 核心概念
## **1. Entry**
  - 代码的入口
  - 打包的入口
  - 单个或多个
可以直接是一个文件
```js
module.exports = {
  entry: './index.js'
}
```
还可以是一个数组
```js
module.exports = {
  entry: ['index.js', 'vendor.js']
}
```
还可以是一个对象
```js
module.exports = {
  entry: {
    index: 'index.js'
  }
}
```

---
## 2. Output
  - 输出
  - 打包成的文件(bundle)
  - 一个或多个
  - 自定义规则
  - 融合CDN
一个
```js
module.exports = {
  entry: 'index.js',
  output: {
    filname: 'index.min.js'
  }
}
```
多个
```js
module.exports = {
  entry: {
    index: 'index.js',
    vendor: 'vendor.js'
  },
  output: {
    filname: '[name].min.[hash:5].js'
  }
  //自定义规则[name]表示entry的name
  //[hash:5] webpack打包过程中出现的md5码
}
```



---
## 3. loaders
靠loaders处理其他类型的文件
处理文件
转化为模块
处理css的loader
```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: 'css-loader'
      }
    ]
  }
}
```
常用loader
编译相关
babel-loader、ts-loader
样式相关
style-loader、css-loader、less-loader、postcss-loader
文件相关
file-loader、url-loader

----
## 4. plugins
压缩代码，混淆，代码分割
参与打包整个过程
打包优化和压缩
配置编译时的变量
极其灵活
```js
const webpack = require('webpack')

module.exports = {
  plugins: [
    new webpack.optimize.UglifyJsPlugin() //压缩混淆
  ]
}
```js
常用Plugins
优化相关
CommonsChunkPlugin 
UglifyjsWebpackPlugin  //压缩混淆
功能相关
ExtractTextWebpackPlugin //打包成单独css文件
HtmlWebpackPlugin //帮助生成html
hotModuleReplacementPlugin //模块热更新
CopyWebpackPlugin //拷贝文件

名词：
Chunk 代码块。
Bundle 打包过以后的
Module 模块

# 编译ES6/7
## **1. Babel**
使用Babel
安装最新版
```js
npm install babel-loader@8.0.0-beta.0 @babel/core
```
普通版本
```js
npm install --save-dev babel-loader babel-core
```
## 2. Babel-presets(针对语法)

es2015
es2016 
es2017
env 一个汇总，包含15-17，以及最近的
自定义的
babel-preset-react  //和React相关的
babel-preset-stage 0-3 //还没有发布的

安装的是最新的loader就可以使用这一句
```js
npm install @babel/preset-env --save-dev
```
安装的是普通版本

```js
npm install babel-preset-env --save-dev
```

targets 可以指定那些语法编译，那些语法不编译
targets.browsers 指定浏览器，还可以指定nodejs版本
```js
targets.browsers: 'last 2 versions' //指定浏览器最后的两个版本
targets.browsers: '> 1%' 大于全球占有率1的浏览器都可以支持
```
数据从github的browserslist项目获得
Can I use

## 3. Babel Polyfill(针对函数和方法)
Generator
set
Map
Array.from
Array.prototype.includes
全局垫片，会对全局污染
为开发应用准备，写一个网站
写vue就不能用Polyfill
```js
npm install babel-polyfill --save
```
import "babel-polyfill"

---js
webpack.config.js
```js
module.exports = {
  entry : {
    app: './app.js'
  },
  output : {
    filename: '[name].[hash:8].js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: [
              ['@babel/preset-env', {
                targets: {
                  browsers: ['> 1%', 'last 2 versions']
                }
              }]
            ]
          }
        },
        exclude: '/node_modules'
      }
    ]
  }
}

```

## 4. Babel Runtime Transform(针对函数和方法)

局部垫片
为开发框架准备
不会污染全局
```bash
npm install babel-plugin-transform-runtime --save-dev
npm install babel-runtime --save
```
不知道@babel这种安装方式是为什么？
```bash
npm install @babel/plugin-transform-runtime --save-dev
npm install @babel/runtime --save
```
这种方法无法实现，不知道为什么，打包后只有2.6k大小
找到解决方法了，增加{ "corejs": 2 }
```js
{
  "presets": [["@babel/env", {
    "targets": {
      "browsers" : ["last 2 versions"] 
    }
  }]],
  "plugins": [["@babel/plugin-transform-runtime", { "corejs": 2 }]]
}
```

# 提取公用代码
**1. 减少代码冗余**

2. 提高加载速度

3. CommonsChunkPlugin 插件提取公用代码。内置插件

webpack.optimize.CommonsChunkPlugin

4. 配置
```js
{
  plugins: [
    new weback.optimize.CommonsChunkPlugin(option)
  ]
}
```js
options配置是什么样的呢：
- options.name or options.names
  - name names 表示Chunk的名称
- options.filename 打包之后的名称
- options.minChunks 
  - 可以是一个数字也可以是一个函数还可以是一个特殊的值Infinity
  - 当他是数字的时候，表示为你要提取代码的出现次数最小是多少
  - Infinity 这个值的时候他不会把任何代码打包进去
  - 还可以是一个函数，可以自定义你的逻辑
- options.chunks 提取代码范围，需要在那几个代码中去提取
- options.children 是不是在entry的子模块中或者你的所有模块中
- options.deepChildren 是不是在entry的子模块中或者你的所有模块中
- options.async 会创建一个异步的工作代码流

需要把webpack安装到本地，因为这个插件是webpack自带的


```js
var webpack = require('webpack')
var path = require('path')

module.exports = {
  entry: {
    'pageA': './src/pageA',
    'pageB': './src/pageB',
    'vendor': 'lodash'
  },
  output: {
    path: path.resolve(__dirname, './dist'),
    filename: '[name].bundle.js',
    chunkFilename: '[name].chunk.js'
  },
  plugins: [
    new webpack.optimize.CommonsChunkPlugin({  //打包自己写的公用代码
      name: 'common',
      minChunks: 2,
      chunks: ['pageA','pageB'] //有bug，这样可以解决
    }),
    new webpack.optimize.CommonsChunkPlugin({  //打包lodash
      names: ['vendor', 'manifest'], //vendor打包的是lodash,manifest打包的是webpack公用代码
      minChunks: Infinity
    })
  ]
}
```

# 代码分割和懒加载

可以通过两种方式懒加载
## **1. webpack** methods
- require.ensure([],callback, [errorCallback], chunkName)
  - [] : dependencies 加载进来并不会执行
  - callback
  - errorCallback 可以省略的第三个参数
  - chunkName
- require.include([])只接受一个参数，但是不执行
  - 当你的子模块都依赖一个第三方模块，就可以把第三方模块放到父模块里面

```js
require.include('./moduleA.js')  //提取公共代码，subPageA和subPageB都引用了moduleA.js

require.ensure(['./subPageA'], function () {  //打包subPageA
  var _ = require('./subPageA')
}, 'subPageA')

require.ensure(['./subPageB'], function () { //打包subPageB
  var _ = require('./subPageB')
}, 'subPageB')


require.ensure(['lodash'], function () { //打包lodash
  var _ = require('lodash')
  _.join(['1', '2'], '3')
}, 'vendor')


export default 'pageA'
```
## 2. ES 2015 Loader spec

```js
import(/* webpackChunkName: 'subpageA' */'./subPageA').then(function(subPageA) {
  console.log(subPageA)
})

/* webpackChunkName: 'subpageA' */是webpack支持的魔法函数
```
pageA.js
```js
import * as _ from 'lodash'

import(/* webpackChunkName: 'subpageA' */'./subPageA').then(function(subPageA) {
  console.log(subPageA)
})
import(/* webpackChunkName: 'subpageB' */'./subPageB').then(function(subPageA) {
  console.log(subPageB)
})

export default 'pageA'
```

## 3. 如果我们想把异步加载进来模块的代码进行提取，除了require.include('./moduleA.js') ，还可以用webpack.config.js完成

```js
var webpack = require('webpack')
var path = require('path')

module.exports = {
  entry: {
    'pageA': './src/pageA',
    'pageB': './src/pageB',
    'vendor': 'lodash'
  },
  output: {
    path: path.resolve(__dirname, './dist'),
    publicPath: './dist/',
    filename: '[name].bundle.js',
    chunkFilename: '[name].chunk.js'
  },
  plugins : [
    new webpack.optimize.CommonsChunkPlugin({  //打包lodash
      async: 'async-common',  //处理异步加载进来的代码
      children: true, //entry模块的子模块
      minChunks: 2
    }),
    new webpack.optimize.CommonsChunkPlugin({  //打包lodash
      names: ['vendor', 'manifest'],
      minChunks: Infinity
    })
  ]
  // plugins: [
  //   new webpack.optimize.CommonsChunkPlugin({  //打包自己写的公用代码
  //     name: 'common',
  //     minChunks: 2,
  //     chunks: ['pageA','pageB'] //有bug，这样可以解决
  //   }),
  //   new webpack.optimize.CommonsChunkPlugin({  //打包lodash
  //     names: ['vendor', 'manifest'],
  //     minChunks: Infinity
  //   })
  // ]
}
```

# 处理css

## **1. style**-loader
创建标签，如何把css放到html中

style-loader
style-loader/url
style-loader/useable

style-loader
```js
var path = require('path')

module.exports = {
  entry: {
    app: './src/app.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    publicPath: './dist/',
    filename: '[name].bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.css/,
        use: [
          {
            loader: 'style-loader' //这种常用
            //loader: 'style-loader/url'  //这种方法比较小众，大家了解就好了
          },
          {
            loader: 'css-loader' //这种常用
            //loader: 'file-loader' //这种方法比较小众，大家了解就好了，这里还要安装file-loader
          }
        ]
      }
    ]
  }
}

```

---
style-loader/useable
```js
module: {
    rules: [
      {
        test: /\.css/,
        use: [
          {
            loader: 'style-loader/useable' 
          },
          {
            loader: 'css-loader' 
          }
        ]
      }
    ]
  }
```
app.js

```bash
import base from './css/base.css'
 
base.use() //引入
base.unuse() //删除
```

---
配置选项
- options
  - insertAt(插入位置)
  - insertInto(插入到dom)
  - singleton(是否使用一个style标签)
  - transform (转化，浏览器环境下，插入页面前)
webpack.config.js
```js
  module: {
    rules: [
      {
        test: /\.css/,
        use: [
          {
            loader: 'style-loader', 
            options: {
              insertInto: '#app',
              singleton: true,
              transform: './css.transform.js'  
            }
          },
          {
            loader: 'css-loader' 
          }
        ]
      }
    ]
  }
```
css.transform.js
``` 这个是浏览器加载的时候执行，可以处理css，但是我不知道有什么用。
module.exports = function (css) {
  console.log(css)
  return css
}
```

## 2. css-loader
如何让js一个import引入css
怎么使用呢，去官方看，老师教的那个minimze已经弃用了
中文官网还是很久之前的文档，所以这里是英文的
[css-loader](https://webpack.js.org/loaders/css-loader/#localidentname)
配置选项
- options
  - alias (解析的别名)
  - importLoader (@import)
  - Minimze(是否压缩)
  - modules (启用css-modules)

## 3. css-modules css模块化

- :local 可以给定一个本地的样式
- :global 可以给定一个全局的样式
- compose 可以继承一段样式
- compose ... from path 从某一个文件引入样式

4. less cass 3-12节
很简单只需要安装所需要的loader
```bash
npm install less-loader less --save-dev
npm install sass-loader node-sass --save-dev
```
编译less
```js
var path = require('path')

module.exports = {
  entry: {
    app: './src/app.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    publicPath: './dist/',
    filename: '[name].bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.less$/,
        use: [
          {
            loader: 'style-loader',
            options: {
              singleton: true,
            }
          },
          {
            loader: 'css-loader'
          },
          {
            loader: 'less-loader'
          }
        ]
      }
    ]
  }
}
```

# 提取css

**1. extract**-loader

## 2. ExtractTextWebpackPlugin 主流
安装
```bash
npm install extract-text-webpack-plugin
```
```js
var path = require('path')
var ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')


module.exports = {
  entry: {
    app: './src/app.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    publicPath: './dist/',
    filename: '[name].bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.less$/,
        use: ExtractTextWebpackPlugin.extract({
          fallback: {
            loader: 'style-loader',
            options: {
              singleton: true,
            }
          },
          use: [
            {
              loader: 'css-loader'
            },
            {
              loader: 'less-loader'
            }
          ]
        })
      }
    ]
  },
  plugins: [
    new ExtractTextWebpackPlugin({
      filename: '[name].min.css'
    })
  ]
}

```
提取出来的文件要自己引入到页面

# PostCSS
安装 ：
```js
postcss
postcss-loader
Atuoprefixer //帮助你写各个浏览器之间的前缀
postcss-cssnano  //帮助我们压缩css
postcss-cssnext  //使用最新的css语法：CSS Variables, custom selectors, calc()
```
npm:
```bash
npm install postcss postcss-loader autoprefixer cssnano postcss-cssnext
```
本节的代码
```js
var path = require('path')
var ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')


module.exports = {
  entry: {
    app: './src/app.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    publicPath: './dist/',
    filename: '[name].bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.less$/,
        use: ExtractTextWebpackPlugin.extract({
          fallback: {
            loader: 'style-loader',
            options: {
              singleton: true,
            }
          },
          use: [
            {
              loader: 'css-loader'
            },
            {
              loader: 'postcss-loader', //引入的位置必须是css-loader之后，less预处理语音之前
              options: {
                ident: 'postcss', //表明后面的插件是给postcss使用的
                plugins: [
                  // require('autoprefixer')(),
                  require('postcss-cssnext')()  //这个里面包含了autoprefixer
                ]
              }
            },
            {
              loader: 'less-loader'
            }
          ]
        })
      }
    ]
  },
  plugins: [
    new ExtractTextWebpackPlugin({
      filename: '[name].min[hash:5].css'
    })
  ]
}


```
## browserslist:
所有插件共用
package.json
```js
"browserslist": [
  ">= 1%",
  "last 2 versions"
]
```
.browserslistrc

---
postcss-import
postcss-url
postcss-assets
后面会讲到

# Tree Shaking

Webpack.optimize.uglifyJs
```js
var webpack = require('webpack')

plugins: [
    new webpack.optimize.UglifyJsPlugin()
  ]
```

lodash 要借助别的插件才可以完成Tree Shaking
```
npm install babel-plugin-lodash --save-dev
npm install babel-loader babel-core babel-preset-env
```
webpack.config.js
```js
module: {
  rules: [
    {
      test: /\.js$/,
      use: [
        {
          loader: 'babel-loader',
          options: {
            presets: ['env'],
            plugins: ['lodash']
          }
        }
      ]
    }
  ]
}
```

新建了个ces文件夹，明天里面搞

找到问题了，如果用babel 6就要安装
```bash
npm install babel-loader@7 babel-core babel-preset-env css-loader@1 //css-loader是上节用的
```
安装babel 7
```bash
npm install babel-loader @babel/core @babel/preset-env css-loader //新版本方便

```
本节代码
```js
var path = require('path')
var webpack = require('webpack')
module.exports = {
  entry: {
    app: './index.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    publicPath: './dist/',
    filename: '[name].bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['env'],
            plugins: ['lodash']
          }
        }
      }
    ]
  },
  plugins: [
    new webpack.optimize.CommonsChunkPlugin({  
      names: ['manifest'], //manifest打包的是webpack公用代码
      minChunks: Infinity
    }),
    new webpack.optimize.UglifyJsPlugin() //Tree Shaking，Webpack.optimize.uglifyJs
  ]
}

```

# CSS Tree Shaking

## Purify CSS
很多打包工具都可以用，比如gulp
针对webpack，purifycss-webpack 
- options
  - paths: glob.sync([])
  - `npm install glob-all --save-dev` //处理多路径

```
npm install purifycss-webpack glob-all
```
新版本还要安装purify-css
```
npm install purify-css
```

```js
var path = require('path')
var ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
var webpack = require('webpack')
var Purifycss = require('purifycss-webpack')
var glob = require('glob-all')

module.exports = {
  entry: {
    app: './src/app.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    publicPath: './dist/',
    filename: '[name].bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.less$/,
        use: ExtractTextWebpackPlugin.extract({
          fallback: {
            loader: 'style-loader',
            options: {
              singleton: true,
            }
          },
          use: [
            {
              loader: 'css-loader'
            },
            {
              loader: 'postcss-loader', //引入的位置必须是css-loader之后，less预处理语音之前
              options: {
                ident: 'postcss', //表明后面的插件是给postcss使用的
                plugins: [
                  require('autoprefixer')(),
                  // require('postcss-cssnext')()  //这个里面包含了autoprefixer
                ]
              }
            },
            {
              loader: 'less-loader'
            }
          ]
        })
      },
      {
        test: /\.js$/,
        use: {
          loader: 'babel-loader',
          options: {
            presets: ['env'],
            plugins: ['lodash']
          }
        },
        exclude: '/node_modules'
      }
    ]
  },
  plugins: [
    new ExtractTextWebpackPlugin({
      filename: '[name].min[hash:5].css'
    }),
    new Purifycss({
      paths: glob.sync([
        path.join(__dirname, './*.html') //本节代码
      ])
    }),
    new webpack.optimize.UglifyJsPlugin()
  ]
}

```

# 文件处理
## 1. 图片处理
css中引入的图片
file-loader


自动合成雪碧图，减少http请求
postcss-sprites

压缩图片
img-loader
小图片可以使用Base64编码
url-loader

字体文件

第三方js库 CDN地址

## 首先安装前面所提到的模块
```bash
npm install file-loader url-loader img-loader postcss-sprites
//放到一起安装会出错?有毒,多安装两遍就好了，postcss-sprites单独安装
```
新版本还要安装
```bash
npm install imagemin imagemin-mozjpeg 

```
研究了一小时的bug，原来是我的图片有问题，哔了狗，500px下的图片。。。

```js
var path = require('path')
var ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
var imagemin = require('imagemin');
module.exports = {
  entry: {
    app: './src/app.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    publicPath: './dist/',
    filename: '[name].bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.less$/,
        use: ExtractTextWebpackPlugin.extract({
          fallback: {
            loader: 'style-loader',
            options: {
              singleton: true
            }
          },
          use: [
            {
              loader: 'css-loader'
            },
            {
              loader: 'less-loader'
            }
          ]
        })
      },
      {
        test: /\.(jpe?g|png|gif|svg)$/,
        use: [
          /*{ // file-loader 把图片打包
            loader: 'file-loader',
            options: {
              name: '[name].img.[hash:5].[ext]',
              outputPath: 'assets',
              publicPath: 'assets'
            }
          }*/
          { //可以将图片转换为bash64编码格式，厉害了，而且配置项和file-loader一样
            loader: 'url-loader',
            options: {
              limit: 10000,
              name: '[name].img.[hash:5].[ext]',
              outputPath: 'assets',
              publicPath: 'assets'
            }
          }, 
          {
            loader: "img-loader",
            options: {
              plugins: [
                require("imagemin-mozjpeg")({
                  quality: 30 // the quality of zip
                })
              ]
            }
          }
        ]
      }
    ]
  },
  plugins: [
    new ExtractTextWebpackPlugin({
      filename: '[name].min.css'
    })
  ]
}
```
需要另外安装
```bash
npm i imagemin-webp imagemin-svgo imagemin-mozjpeg imagemin-pngquant imagemin-optipng imagemin-gifsicle imagemin cwebp-bin
```
其中webp打包的时候报过一个错
```error
ERROR in ./src/css/base.less
Module build failed: ModuleBuildError: Module build failed: Error: spawn /mnt/f/webpack/4-1/node_modules/cwebp-bin/vendor/cwebp ENOENT
```
安装cwebp-bin解决
```bash
npm i cwebp-bin
```


使用images-webapack-loader

```js
{
  loader: "image-webpack-loader",
  options: {
    mozjpeg: {
      quality: 30
    },
    // optipng.enabled: false will disable optipng
    optipng: {
      enabled: false,
    },
    pngquant: {
      quality: '65-90',
      speed: 4
    },
    gifsicle: {
      interlaced: false,
    },
    webp: { // 转换为webp格式文件
      quality: 60
    }
  }
}
```

没搞懂webp是干什么的
好像webp没什么用啊，头疼

> 2019年3月8日02:15:55 说一下搭建webpack 4 中使用image-webpack-loader遇到的坑：安装这个插件的时候我退出了一下导致很多类似于上面的*-bin文件没有安装，而且卸载这个插件重新安装也不会装这些东西，不知道是什么坑,解决办法就是去别的目录重新执行此命令看少装了什么，重新npm i 安装即可


## imagemin-pngquant 压缩png图片
嗯，bug出现的问题是npm安装的时候我退出了，要安装个什么pngquant-bin,前面那个webp-bin也是我自己退出了，毕竟以前没遇到过这种问题
```
npm i pngquant-bin
```
还有就是压缩图片还要用上file-loader,url-loader

## postcss-sprites 雪碧图
这个是postcss的一个雪碧图插件
```js
{
  loader: 'postcss-loader',
  options: {
    ident: 'postcss',
    plugins: [
      require('postcss-sprites')(), //雪碧图
      //Atuoprefixer 帮助你写各个浏览器之间的前缀
      require('postcss-cssnext')()  //使用最新的css语法，这个里面包含了autoprefixer
    ]
  }
}
```

## 本节代码：
```js
var path = require('path')
var ExtractTextWebpackPlugin = require('extract-text-webpack-plugin')
var imagemin = require('imagemin');
module.exports = {
  entry: {
    app: './src/app.js'
  },
  output: {
    path: path.resolve(__dirname, 'dist'),
    publicPath: './dist/',
    filename: '[name].bundle.js'
  },
  module: {
    rules: [
      {
        test: /\.less$/,
        use: ExtractTextWebpackPlugin.extract({
          fallback: {
            loader: 'style-loader',
            options: {
              singleton: true
            }
          },
          use: [
            {
              loader: 'css-loader'
            },
            {
              loader: 'postcss-loader',
              options: {
                ident: 'postcss',
                plugins: [
                  require('postcss-sprites')(),
                  // require('postcss-sprites')(),
                  require('postcss-cssnext')()
                ]
              }
            },
            {
              loader: 'less-loader'
            }
          ]
        })
      },
      {
        test: /\.(gif|png|jpe?g|svg)$/i,
        use: [
          /*{ // file-loader 把图片打包
            loader: 'file-loader',
            options: {
              name: '[name].img.[hash:5].[ext]',
              outputPath: 'assets',
              publicPath: 'assets'
            }
          }*/
          { //可以将图片转换为bash64编码格式，厉害了，而且配置项和file-loader一样
            loader: 'url-loader',
            options: {
              limit: 100,
              name: '[name].img.[hash:5].[ext]',
              outputPath: 'assets',
              publicPath: 'assets'
            }
          }, 
          {
            loader: "img-loader",
            options: {
              /*plugins: [
                // require("imagemin-mozjpeg")({  //压缩jpg
                //   quality: 60 // the quality of zip
                // }),
                // require("imagemin-webp")({  //压缩为webp
                //   quality: 80 // the quality of zip
                // })
              ]*/
                plugins: [
                  require("imagemin-pngquant")({
                    quality: [0.6, 0.7]
                  })
                ]
            }
          }
        ]
      }
    ]
  },
  plugins: [
    new ExtractTextWebpackPlugin({
      filename: '[name].min.css'
    })
  ]
}
```

# 字体文件
`file-loader` 这个或 `url-loader` 都可以
很简单，新加配置项
这个就是这部分添加的，完整文件参考上面
```js
{
  test: /\.(eot|svg|ttf|woff?)$/,
  use: [
    {
      loader: 'url-loader',
      options: {
        limit: 1000,
        name: '[name].font.[hash:6].[ext]',
        outputPath: 'assets',
        publicPath: 'assets'
      }
    }
  ]
}
```

# 第三方库

`webpack.providePlugin`
`imports-loader`
`window`
全局注入`npm`模块
```
plugins: {
  new webpack.ProvidePlugin({
    $: 'jquery' //这里的模块就可以全局注入，
  })
}
```
如果npm没有安装这个模块，但是本地有，就可以这样
```
module: {
  rules: [
    resolve: {
      alias: {
        jquery$: path.resolve(__dirname, 'src/jquery.min.js')
        //这个$不知道干啥的
      }
    }
  ]
}
```
# webpack 处理 html文件
## 自动生成HTML , 场景优化
`HtmlWebpackPlugin` 插件简介
- `options`
  - `template` 这个项目的模板文件所在位置，我们的html就在根目录
  - `filename` 指定输出目录，目录，文件名，都可以
  - `minify` 是否压缩生成html文件
  - `chunks` 指定entry的chunk，不然的话多页面应用就会把所有的加进来
  - `inject` 这个默认是true，改为false就是手动插入

安装 HtmlWebpackPlugin
```bash
npm install html-webpack-plugin --save-dev
```
webpack.config.json 文件写入
```js
var HtmlWebpackPlugin = require('html-webpack-plugin') //头部引入

plugins: [
  new HtmlWebpackPlugin({
    filename: 'index.html', //目录，文件名，都可以
    template: './index.html', //这个项目的模板文件所在位置，我们的html就在根目录
    //inject: false ,//这个默认是true，改为false就是手动插入
    chunks: ['app'], //指定entry的chunk，不然的话多页面应用就会把所有的加进来
    minify: { //借助了第三方实现html-minify
    collapseWhitespace: true //去除空格
  }
  })
]

```
//顺便还要去除，output中的路径，因为index文件到同级目录了，所以就不用增加这段代码了

```js
output: {
  ~~publicPath: './dist/',~~
},
```

# HTML中引入的图片

- `html-loader`
- `options` 选项
  - attrs: [img: src] img表示html标签，右边表示属性

安装`html-loader`
```bash
npm install html-loader
```
index.html：
```html
<img src="" data-src="">
```
webpack.config.json:
```js
module: {
  rules: [
    {
      test: /\.html$/,
      use: [
        {
          loader: 'html-loader',
          options: {
            attrs: ['img:src', 'img:data-src']//默认是img:src,如果有其他需求可以增加
          }
        }
      ]
    } 
  ]
}
```

更改，删除的地方
webpack.config.js
```js
output: {
  publicPath: '/' //修改为根路径
}
//其他的publicPath全部删除即可，这一个就ok了，以前因为没有打包index.html所以才很麻烦
```

即使没有html-loader,还可以用require的方式引入图片
```html
  <img src='${require("./src/assets/imgs/2.jpg)"}' alt="图片2">
```


# 配合优化(提前载入webpack加载代码，减少http请求)

`inline-manifest-webpack-plugin` 把webpack生成的代码插入页面，和html-loader配合的时候会有bug，所以就学习第二种

`html-webpack-inline-chunk-plugin` 通过这个插件可以选择各种各样的chunk，然后插入到页面中

```bash
npm install html-webpack-inline-chunk-plugin --save-dev
```
```
npm install babel-loader @babel/core @babel/preset-env --dev
```

webpack.config.js
```js
module: {
  rules: [
      { //位置是最顶部，不知道放到后面会有影响不
        test: /\.js$/,
        use: [
          {
            loader: 'babel-loader', 
            options: {
              presets: ['@babel/preset-env'],
            }
          }
        ]
      }
  ]
}
```
```js
var HtmlWebpackPlugin = require('html-webpack-plugin')
var HtmlInlinkChunkPlugin = require('html-webpack-inline-chunk-plugin')

plugins: [
    new webpack.optimize.CommonsChunkPlugin({
      name: 'manifest'
    }),
    new HtmlInlinkChunkPlugin({
      inlineChunks: ['manifest']
    }),
  ]
```

# 5-1 搭建开发环境

 **webpack watch mode**
```bash
webpack -watch
webpack -w
```
安装一个自动清除目录的工具
```bash
npm install clean-webpack-plugin 
```
webpack.config.js
```js
var CleanWebpackPlugin = require('clean-webpack-plugin')

plugins: [
  new CleanWebpackPlugin(['dist']) //这里是要清除的目录
]
```
使用`webpack -w`
```bash
webpack -w --progress --display-reasons --color
```
- progress 运行的过程

## **webpack-dev-server**
**官方提供的开发服务器**
- live reloading 项目变化帮你刷新浏览器
- 打包文件？NO
  - 是在运行的内存中，所以还是需要自己打包
- 路径重定向
  - 没有理解
-  https 支持
-  浏览器中显示编译错误，很有用
-  接口代理
-  模块热更新
  - 不刷新浏览器的情况下，更新代码，这个厉害了

**配置**
- devServer 
  - inline
  - contentBase 
  - port
  - historyApiFallback
  - https
  -  proxy
  -  hot 模块热更新，还要写额外代码
  -  openpage 指定最先打开的页面
  -  lazy 启动的时候不会打包任何东西，只有访问的时候才会打包
  -  overlay 遮罩，错误提示

**开始使用**

安装`webpack-dev-server`
```bash
npm install webpack-dev-server@2 //3以上需要webpack@4支持，所以这里要降级
```
webpack.config.js
```js
module.exports = {
  devServer: {
    port : 9001 //指定端口
  }
}
```
package.json
```json
"scripts" : {
  "start:dev": "webpack-dev-server --open"
}
```
命令行执行
```bash
npm run start:dev
```
不配置package.json，可以这样
```bash
./node_modules/.bin/webpack-dev-server
```
**原来package.json script里面找模块是从node_modules中查找的，这是npm的查找规则决定的**

```js
module.exports = {
  devServer: {
    // inline: false, //目前用不到
    // historyApiFallback: true, //嗯，目前用不到，用到再说
    port: 9001
  },
}
```

## **Proxy** 代理远程接口请求
自己集成了http-proxy-middleware
- options
  - target 指定代理地址
  - changeOrigin 改变源，到你的url
  - headers 增加http请求头，
  - logLevel 帮助调试，显示代理情况
  - pathRewrite 帮助重定向

**使用**
webpack.config.js
```js
module.exports = {
  devServer: {
    proxy: {
      '/comments' : { //匹配地址，凡是遇到/comments的都会被转为下面的路径
      target: 'https://m.weibo.com',
      changeOrigin: true 
      }
    }
  },
  plugins: [
    new webpack.ProvidePlugin({
      $: 'jquery' //这里的模块就可以全局注入
    })
  ]
}
```
app.js
```js
$.get('/comments/hotflow', { //微博评论接口，这里$.get自然是jquery了
  id : 4331316696337556,
  mid : 4331316696337556,
  max_id_type : 0
}, function(data) {
  console.log(data.data.data) //到这里就可以结束了，下面的只是插入页面
  var msg = data.data.data
  for (var i = 0 ; i < msg.length; i++) {
    console.log(msg[i].text)
    $('.pl').append("<div>" + msg[i].text + "<div/>")
  }
})
```
**其他配置项**
headers

app.js  
```js
$.get('/msg/index', {
  format: 'cards'
}, function (data) {
  console.log(data)
})
```
以上写法，微博会有验证，无法通过，可以用proxy发送头信息
```js
module.exports = {
  devServer: {
    proxy: {
      '/' : { //匹配地址，凡是遇到/comments的都会被转为下面的路径
      target: 'https://m.weibo.com',
      changeOrigin: true ,
      headers: { //发送头信息，这里的cookie是微博的验证
        'Cookie': 'ALF=1550807798; SCF=Ajv5Mdqcv9zNmq8TEe8b9Jd5bIERX_ZZAEkFyb_x_x5DdNtQFxGLsekcUUeJ5LOavTb_ruqz1FbarpstC8U9w8o.; SUB=_2A25xQ4KVDeRhGeVJ41IW8i7Kwj-IHXVSzy7drDV6PUJbktAKLU7lkW1NT8d4sQ6RoEu17F8RbjIXYg74GYrrPENl; SUBP=0033WrSXqPxfM725Ws9jqgMF55529P9D9WFe-YDR22UIh60JG0EfcL8h5JpX5K-hUgL.FoeN1h5Neo5c1Ke2dJLoIEYLxKnL1heLB.BLxKMLB-eL1K2LxK-LBo5LBo8bUCH81FHWSb-ReCH8SC-4eEHWx5tt; SUHB=0LcilU3V_eaMdU; SSOLoginState=1548219077; MLOGIN=1; _T_WM=60e134ae82c5c5d1a5652f6a212d78f0; WEIBOCN_FROM=1110003030; XSRF-TOKEN=4d3335; M_WEIBOCN_PARAMS=luicode%3D10000011%26lfid%3D100505102803%26uicode%3D10000011%26fid%3D102803'
        }
      }
    }
  }
}
```
pathRewrite
```js
module.exports = {
  devServer: {
    proxy: {
      '/' : {  //直接匹配/
        target: 'https://m.weibo.com',
        changeOrigin: true ,
        pathRewrite: {
      '^/comments' : '/api/comments' //把/comments的地址都替换，不过我的这个案例没用使用，和前面app.js例子会出问题
        }
      }
    }
  },

}
```
## **Module Hot Reloading**
**模块热更新**
保持应用的数据状态
节省调试时间
样式调试更快

**devServer,已经集成，直接使用**
```js
devServer: {
  hot: true
}
```
除此之外还要使用
`webpack.HotModuleReplacementPlugin`
想清晰看到模块的相对路径可以使用
`webpack.NamedModulesPlugin`

```js
plugins: [
  new webpack.HotModuleReplacementPlugin()
  new webpack.NamedModulesPlugin()
]
```
如果js需要热加载，就要自己去写规则，vue，angular这种框架的loader都写好了，自己写页面感觉没必要
* module.hot
* module.hot.accept
* module.hot.decline

app.js 
```js
if (module.hot) {  //简单的用法，引入其他模块还要增加代码
  module.hot.accept()
}
```
## **express+webpack-dev-middleware**

# Source Map调试
主要就是帮你定位代码的位置，比如打包后就变成一行，启用Devtool就可以定位到要调试代码的位置
插件(更加灵活):
```js
webpack.SouceMapDevToolPlugin
webpack.EvalSourceMapDevToolPlugin

```
**Devtool**
Development(开发环境使用)
- options
  - eval
  - eval-source-map
  - cheap-eval-source-map
  - cheap-module-eval-source-map

Production(生产环境下)
- options
  - source-map
  - hidden-source-map
  - nosource-source-map
  
每个选项的生成时间都不同

**css使用source-map注意**
除了开启source-map之外，每个css-loader都要启用`options.sourceMap = true`

**使用source-map**
webpack.config.js
```js
module.exports = {
  //devtool: 'eval', 这个速度最快但是看到的文件不够清晰，会带有webpack自己的代码
  //devtool: 'source-map', //这个虽然耗时，不过还行
  devtool: 'cheap-module-source-map' //感觉这个和source-map区别不大，估计还没遇到问题把
} 
```
如果开启最小化代码`new webpack.optimize.UglifyJsPlugin()`就会出现问题，课程是暂时注释

css用到的loader都要开启sourceMap
```js
{
  loader: 'css-loader',
  options: {
    sourceMap: true
  }
}
```
有个style-loader的bug，就是开启`singleton: true` 开启sourceMap还是不行，新版style-loader已经修复，我没有这个问题

# Eslint 检查代码格式
所需要的插件
```js
eslint
eslint-loader
eslint-plugin-html  处理html里面的script
eslint-friendly-formatter  友好的看到输出错误
```
配置
webpack config
.eslintrc.*
package.json 中的 eslintConfig
使用那种方式验证eslint
[JavaScript Standard Style](https://standardjs.com/) 
需要的插件
```js
eslint-config-standard
eslint-plugin-promise
eslint-plugin-standard
eslint-plugin-import
eslint-plugin-node
```
如果不使用这个插件可以去github搜`eslint-config-xxx`

- eslint-loader使用注意
  - options.failOnWarning 设为true，如果有错误就不会通过编译
  - options.failOnError 定义验证错误不会通过编译
  - options.formatter 
  - options.outputReport
- devServer.overlay 开启后就可以在浏览器中看到错误

**开始使用**
安装需要的模块
```sh
npm install eslint eslint-loader eslint-plugin-html eslint-friendly-formatter --save-dev
```
增加配置代码
webpack.config.js
```js
module: {
  rules: [
    {
      test: /\.js$/,
      use: [
        {
          loader: 'babel-loader',
          //include: [path.resolve(__dirname, 'src')],
          //exclude: [path.resolve(__dirname, 'src/lib')]
          options: {
            presets: ['@babel/preset-env']
          }
        },
        { //必须添加到babel-loader之后
          loader: 'eslint-loader',
          options: {
            formatter: require('eslint-friendly-formatter')
          }
        }
      ],
    }
  ]
}
```
.eslintrc.js
```js
module.exports = {
  root: true, //表面自己是根目录
  extends: 'standard', //使用standard的标准
  plugins: [],
  env: { //环境
    browser: true, //浏览器环境
    node: true //node环境
  },
  globals: {
    $: true
  },
  rules: { //可以覆盖standard的标准，自定义自己的规则
    //indent: ['error', 4] //将缩进自定义为4个空格 
    //'eol-last': ['error', 'never'] //结尾需要换行取消
  }
}
```
安装`standard`所需模块
```sh
npm install eslint-config-standard eslint-plugin-promise eslint-plugin-node eslint-plugin-import eslint-plugin-standard --save-dev
```
个人觉得这个显示到html页面上的错误有点难看，不知道vue-cli使用的是什么



# 开发环境和生产环境
需求分析：
**开发环境**
模块热更新
sourceMap
接口代理
代码规范检查
**生产环境**
提取公用代码
压缩混淆
文件压缩或Base64 编码
去除无用的代码

**共同点**
同样的入口
同样的代码处理(loader处理) 所有loader
同样的解析配置 开发和打包的一致性

**如何区分呢**
就是webpack-merge

webpack.dev.conf.js  //开发环境
webpack.prod.conf.js  //生产环境
webpack.common.conf.js  //共同点