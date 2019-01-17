---
title: webpack
date: 2019-01-06 20:29:16
tags: [webpack]
categories:
---

[webpack版本更迭](https://github.com/webpack/webpack/releases)
[社区投票](https://webpack.js.org/vote)
[官方文档](https://webpack.docschina.org/concepts/)
# 模块化开发


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

```
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
```
import { name as myNamed, named } from 'src/mylib'
```
这样的写法和这个貌似是一样的
```
import { name : myNamed, named } from 'src/mylib'
```
拿到全部方法，mylib可以使用他
```
import * as mylib from 'src/mylib'
```
只是加载进来，没有任何引用
```
import 'src/mylib'
```
---
**export, export default**
- export, export default 均可到处常量，函数，文件，模块等
- export 一个文件可以有多个，
- export default一个文件只能有一个
- 通过export方式导出，在导入时要加{},export default 则不用
- export 能直接导出变量表达式，export default则不行
```
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
```
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
```
module.exports = {
  entry: './index.js'
}
```
还可以是一个数组
```
module.exports = {
  entry: ['index.js', 'vendor.js']
}
```
还可以是一个对象
```
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
```
module.exports = {
  entry: 'index.js',
  output: {
    filname: 'index.min.js'
  }
}
```
多个
```
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
```
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
```
const webpack = require('webpack')

module.exports = {
  plugins: [
    new webpack.optimize.UglifyJsPlugin() //压缩混淆
  ]
}
```
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
```
npm install babel-loader@8.0.0-beta.0 @babel/core
```
普通版本
```
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
```
npm install @babel/preset-env --save-dev
```
安装的是普通版本

```
npm install babel-preset-env --save-dev
```

targets 可以指定那些语法编译，那些语法不编译
targets.browsers 指定浏览器，还可以指定nodejs版本
```
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
```
npm install babel-polyfill --save
```
import "babel-polyfill"

---js
webpack.config.js
```
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
```
npm install babel-plugin-transform-runtime --save-dev
npm install babel-runtime --save
```
不知道@babel这种安装方式是为什么？
```
npm install @babel/plugin-transform-runtime --save-dev
npm install @babel/runtime --save
```
这种方法无法实现，不知道为什么，打包后只有2.6k大小
找到解决方法了，增加{ "corejs": 2 }
```
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
```
{
  plugins: [
    new weback.optimize.CommonsChunkPlugin(option)
  ]
}
```
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


```
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

```
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

```
import(/* webpackChunkName: 'subpageA' */'./subPageA').then(function(subPageA) {
  console.log(subPageA)
})

/* webpackChunkName: 'subpageA' */是webpack支持的魔法函数
```
pageA.js
```
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

```
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
```
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
```
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

```
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
```
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
```
npm install less-loader less --save-dev
npm install sass-loader node-sass --save-dev
```
编译less
```
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
```
npm install extract-text-webpack-plugin
```
```
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
```
postcss
postcss-loader
Atuoprefixer //帮助你写各个浏览器之间的前缀
postcss-cssnano  //帮助我们压缩css
postcss-cssnext  //使用最新的css语法：CSS Variables, custom selectors, calc()
```
npm:
```
npm install postcss postcss-loader autoprefixer cssnano postcss-cssnext
```
本节的代码
```
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
browserslist:
所有插件共用
package.json
```
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
```
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
```
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
```
npm install babel-loader@7 babel-core babel-preset-env css-loader@1 //css-loader是上节用的
```
安装babel 7
```
npm install babel-loader @babel/core @babel/preset-env css-loader //新版本方便

```
本节代码
```
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

```
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

```
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
```
npm i cwebp-bin
```


使用images-webapack-loader

```
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