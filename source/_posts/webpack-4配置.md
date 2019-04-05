---
title: webpack 4配置
date: 2019-03-08 22:45:23
tags:
categories:
---

# webpack 配置
<!-- more -->

# 更新后的配置，目前再用的，以前的废弃了
```js
const path = require('path');
const devMode = process.env.NODE_ENV !== 'production'
// const devMode = true
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const webpack = require('webpack');

module.exports = {
  entry: {
    index: './src/pages/index/index.js',
    // main: './src/pages/page1/main.js'
  },
  plugins: [
    new HtmlWebpackPlugin({
      template: './src/pages/index/index.html',
      filename: 'index.html',
      inject: true,
      hash: true,
      chunks: ['vendor','common','runtime','index'],
      minify: process.env.NODE_ENV !== "production" ? false : {
          removeComments: true,
          // collapseWhitespace: true,
          removeAttributeQuotes: true,
      }
    }),
/*    new HtmlWebpackPlugin({
      template: './src/pages/page1/main.html',
      filename: 'main.html',
      inject: true,
      hash: true,
      chunks: ['vendor','common','runtime','main'],
      minify: process.env.NODE_ENV !== "production" ? false : {
          removeComments: true,
          // collapseWhitespace: true,
          removeAttributeQuotes: true,
      }
    }),*/
    new webpack.ProvidePlugin({
      $: 'jquery',
      jQuery: 'jquery'
    })

  ],
  output: {
    filename: devMode ? 'js/[name].[hash:8].js': 'js/[name].[chunkhash:8].js',
    chunkFilename: devMode ? 'js/[name].[hash:8].js': 'js/[name].[chunkhash:8].js',
    path: path.resolve(__dirname, '../dist'),
    publicPath: './'
  },
  resolve: {
    alias: {
      // '@': resolve('src'),
      '@': path.resolve(__dirname, '../src/'),
      '~node': path.resolve(__dirname, '../node_modules/'),
      '~css': path.resolve(__dirname, '../src/css/'),
      // 'common': resolve('src/common')
    }
  },
  optimization: {
    runtimeChunk: 'single',
    splitChunks: {
      cacheGroups: {
          vendor: { // 抽离第三方插件
              test: /node_modules/, // 指定是node_modules下的第三方包
              chunks: 'initial',
              name: 'vendor', // 打包后的文件名，任意命名
              // 设置优先级，防止和自定义的公共代码提取时被覆盖，不进行打包
              priority: 10
          },
          utils: { // 抽离自己写的公共代码，common这个名字可以随意起
              chunks: 'initial',
              name: 'common', // 任意命名
              minSize: 0, // 只要超出0字节就生成一个新包
              minChunks: 2
          }
      }
    }
  },
  module: {
    rules: [
      {
        test: /\.(less|css)$/,
        use: [
          {
            //因为只有style-loader有热更新，所以这么写
            loader: devMode ? 'style-loader' : MiniCssExtractPlugin.loader,
            options: devMode ? {
              sourceMap: true,
            } : {
              publicPath: '../'
            }
          },
          {
            loader: 'css-loader',
            options: {
              sourceMap: true
            }
          },
          {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              sourceMap: true,
              plugins: [
                require('postcss-preset-env')()
              ]
            }
          },
          {
            loader: 'less-loader',
            options: {
              sourceMap: true
            }
          }
        ],
      },
      {
        test: /\.(gif|png|jpe?g|svg)$/i,
        use: [
          {
            loader: 'url-loader',
            // loader: 'file-loader',
            options: {
              limit: 8192,
              name: '[name].img.[hash:5].[ext]',
              outputPath: 'img',
            }
          },
          {
            loader: "image-webpack-loader",
            options: {
              mozjpeg: {
                progressive: true,
                quality: 65
              },
              optipng: {
                enabled: false,
              },
              pngquant: {
                quality: '65-90',
                speed: 4
              },
              gifsicle: {
                interlaced: false,
              }
            }
          }
        ]
      },
      {
        test: /\.(woff|woff2|eot|ttf|otf|svg)$/,
        use: [
          {
            loader: 'file-loader',
            options: {
              name: 'css/font/[name].font.[hash:6].[ext]',
            }
          }
        ]
      },
      {
        test: /\.html$/,
        use: [
          {
            //raw-loader是修改html更新页面，而且必须这么写，不然两个一起用会出现bug，如果只用raw-loader页面就没办法处理Img了
            loader: 'html-loader?config=raw-loader',
            options: {
              attrs: ['img:src', 'img:data-src'],
            }
          },
        ]
      },
      {
        //这里也是踩坑了，必须webpack这么写，然后.babelrc 才会被处理
        test: /\.js$/,
        use: [
          {
            loader: 'babel-loader'
          }
        ]
      }
    ]
  },

};

```
# .babelrc

```json
{
  "presets": ["@babel/preset-env"],
  "plugins": [["@babel/plugin-transform-runtime", { "corejs": 2 }]]
}
```
# webpack.dev.js 主要就是更新了路径把，因为目录全部迁移到build了
```js
const merge = require('webpack-merge');
const common = require('./webpack.common.js');
const path = require('path');

module.exports = merge(common, {
  mode: 'development',
  devtool: 'inline-source-map',
  devServer: {
    contentBase: path.resolve(__dirname, '../src/pages/assets'),
    publicPath: '/',
    hot: true,
    // compress: true,
    // overlay: true
  }
});

```
# webpack.prod.js
```js
const merge = require('webpack-merge');
const common = require('./webpack.common.js');
const path = require('path');

const CleanWebpackPlugin = require('clean-webpack-plugin');
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");
const HtmlWebpackPlugin = require('html-webpack-plugin');
// const UglifyJsPlugin = require("uglifyjs-webpack-plugin");
const TerserPlugin = require('terser-webpack-plugin');
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
// const Purifycss = require('purifycss-webpack')
const PurgecssPlugin = require('purgecss-webpack-plugin')
const glob = require('glob-all')
const copyWebpackPlugin = require("copy-webpack-plugin");

const PATHS = {
  src: path.join(__dirname, '../src')
}

module.exports = merge(common, {
  mode: 'production',
  optimization: {
    minimizer: [
      //这个可以支持es6语法，不过用babel打包后也可以支持？，没测试，当时是babel没设置好，出现了一点bug
      new TerserPlugin(),
      new OptimizeCSSAssetsPlugin({})
    ]
  },
  plugins: [
    new CleanWebpackPlugin(['dist/*'],{
      root: path.resolve(__dirname, '../'), //根目录
      verbose: true, //开启在控制台输出信息
      dry: false,
    }),
    new MiniCssExtractPlugin({
      filename: "css/[name].[contenthash:8].css",
      chunkFilename: "css/[name].[contenthash:8].css",
    }),
    //拷贝静态文件，这个其实还要再改的，我没弄，不过这个改起来很方便
    new copyWebpackPlugin([{
        from: path.resolve(__dirname, "../src/assets"),
        to: './assets',
        // ignore: ['.*']
    }]),
    new copyWebpackPlugin([{
        from: path.resolve(__dirname, "../src/pages/assets"),
        to: './',
        ignore: ['.*']
    }]),
    // new Purifycss({
    //   paths: glob.sync([
    //     path.join(__dirname, '../src/pages/*/*.html'),
    //     path.join(__dirname, '../src/pages/*/*.js')
    //   ]),
    //   purifyOptions: {
    //     whitelist: ['slick.less','slick-theme.less']
    //   }
    // }),

    //css的去重，以前用的purifycss，好像不更新了，这个是新版，后面有个白名单，正则匹配，就是不管里面的class命名的代码
    new PurgecssPlugin({
      paths: glob.sync(`${PATHS.src}/pages/*/*`,  { nodir: true }),
      //白名单css，正则匹配css
      whitelistPatterns: [/^(slick)/]
    }),
  ]
});

```
# package.json
```json
{
  "name": "shop",
  "version": "1.0.0",
  "description": "",
  "main": "webpack.common.js",
  "scripts": {
    "dev": "cross-env NODE_ENV=development webpack-dev-server --host 0.0.0.0 --config build/webpack.dev.js",
    "build": "cross-env NODE_ENV=production webpack --config build/webpack.prod.js"
  },
  "repository": {
    "type": "git",
    "url": "git+https://github.com/DemoorBug/shop.git"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "bugs": {
    "url": "https://github.com/DemoorBug/shop/issues"
  },
  "homepage": "https://github.com/DemoorBug/shop#readme",
  "devDependencies": {
    "@babel/core": "^7.3.4",
    "@babel/plugin-transform-runtime": "^7.3.4",
    "@babel/preset-env": "^7.3.4",
    "@babel/runtime": "^7.3.4",
    "@babel/runtime-corejs2": "^7.3.4",
    "babel-loader": "^8.0.5",
    "clean-webpack-plugin": "^1.0.1",
    "copy-webpack-plugin": "^5.0.0",
    "cross-env": "^5.2.0",
    "css-loader": "^2.1.0",
    "eslint": "^5.15.1",
    "file-loader": "^3.0.1",
    "glob-all": "^3.1.0",
    "html-loader": "^0.5.5",
    "html-webpack-plugin": "^3.2.0",
    "image-webpack-loader": "^4.6.0",
    "imagemin": "^6.1.0",
    "imagemin-gifsicle": "^6.0.1",
    "imagemin-mozjpeg": "^8.0.0",
    "imagemin-optipng": "^6.0.0",
    "imagemin-pngquant": "^7.0.0",
    "imagemin-svgo": "^7.0.0",
    "imagemin-webp": "^5.0.0",
    "img-loader": "^3.0.1",
    "less": "^3.9.0",
    "less-loader": "^4.1.0",
    "mini-css-extract-plugin": "^0.5.0",
    "optimize-css-assets-webpack-plugin": "^5.0.1",
    "postcss-cssnext": "^3.1.0",
    "postcss-loader": "^3.0.0",
    "postcss-preset-env": "^6.6.0",
    "purgecss-webpack-plugin": "^1.4.0",
    "purify-css": "^1.2.5",
    "purifycss-webpack": "^0.7.0",
    "raw-loader": "^1.0.0",
    "style-loader": "^0.23.1",
    "terser-webpack-plugin": "^1.2.3",
    "uglifyjs-webpack-plugin": "^2.1.2",
    "url-loader": "^1.1.2",
    "webpack": "^4.29.6",
    "webpack-cli": "^3.2.3",
    "webpack-dev-server": "^3.2.1",
    "webpack-merge": "^4.2.1"
  },
  "dependencies": {
    "bootstrap": "^3.4.1",
    "jquery": "^3.3.1",
    "lodash": "^4.17.11",
    "minireset.css": "0.0.4",
    "normalize.css": "^8.0.1",
    "slick-carousel": "^1.8.1"
  },
  "browserslist": [
    "> 1%",
    "last 2 versions",
    "not ie <= 8"
  ]
}

```


# webpack.common.js 后面的都弃用

```js

const path = require('path');
const devMode = process.env.NODE_ENV !== 'production'
// const devMode = true
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const HtmlWebpackPlugin = require('html-webpack-plugin');


module.exports = {
  //入口文件依据目录随时添加
  entry: {
    index: './src/pages/index/index.js',
    main: './src/pages/page1/main.js'
  },
  plugins: [
    //HtmlWebpackPlugin这个也是依据入口文件随时添加，我这样写很麻烦，要弄个自动化的
    new HtmlWebpackPlugin({
      template: './src/pages/index/index.html',
      filename: 'index.html',
      inject: true,
      hash: true,
      chunks: ['vendor','common','runtime','index'],
      minify: process.env.NODE_ENV !== "production" ? false : {
          removeComments: true,
          // collapseWhitespace: true,
          removeAttributeQuotes: true,
      }
    }),
    new HtmlWebpackPlugin({
      template: './src/pages/page1/main.html',
      filename: 'main.html',
      inject: true,
      hash: true,
      chunks: ['vendor','common','runtime','main'],
      minify: process.env.NODE_ENV !== "production" ? false : {
          removeComments: true,
          // collapseWhitespace: true,
          removeAttributeQuotes: true,
      }
    }),

  ],
  output: {
    filename: devMode ? 'js/[name].[hash:8].js': 'js/[name].[chunkhash:8].js',
    chunkFilename: devMode ? 'js/[name].[hash:8].js': 'js/[name].[chunkhash:8].js',
    path: path.resolve(__dirname, '../dist'),
    publicPath: './'
  },
  optimization: {
    runtimeChunk: 'single',
    splitChunks: {
      cacheGroups: {
          vendor: { // 抽离第三方插件
              test: /node_modules/, // 指定是node_modules下的第三方包
              chunks: 'initial',
              name: 'vendor', // 打包后的文件名，任意命名    
              // 设置优先级，防止和自定义的公共代码提取时被覆盖，不进行打包
              priority: 10
          },
          utils: { // 抽离自己写的公共代码，common这个名字可以随意起
              chunks: 'initial',
              name: 'common', // 任意命名
              minSize: 0, // 只要超出0字节就生成一个新包
              minChunks: 2
          }
      }
    }
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          {
            loader: devMode ? 'style-loader' : MiniCssExtractPlugin.loader,
            options: {
              sourceMap: true,
              //单独给css里面的路径设置规则，防止和html里面的img路径冲突
              publicPath: '../'
            }
          },
          {
            loader: 'css-loader',
            options: {
              sourceMap: true
            }
          },
          {
            loader: 'postcss-loader',
            options: {
              ident: 'postcss',
              sourceMap: true,
              plugins: [
                require('postcss-preset-env')()
              ]
            }
          }
        ],
      },
      {
        test: /\.(gif|png|jpe?g|svg)$/i,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192,
              name: '[name].img.[hash:5].[ext]',
              outputPath: 'img',
            }
          },
          {
            loader: "image-webpack-loader",
            options: {
              mozjpeg: {
                progressive: true,
                quality: 65
              },
              optipng: {
                enabled: false,
              },
              pngquant: {
                quality: '65-90',
                speed: 4
              },
              gifsicle: {
                interlaced: false,
              }
            }
          }
        ]
      },
      {
        test: /\.(woff|woff2|eot|ttf|otf|svg)$/,
        use: [
          {
            loader: 'file-loader',
            options: {
              name: 'css/font/[name].font.[hash:6].[ext]',
            }
          }
        ]
      },
      {
        test: /\.html$/,
        use: [
          {
            loader: 'html-loader',
            options: {
              attrs: ['img:src', 'img:data-src'],
            }
          }
        ]
      }
    ]
  },  

};
```

# webpack.dev.js
```js
const merge = require('webpack-merge');
const common = require('./webpack.common.js');
module.exports = merge(common, {
  mode: 'development',
  devtool: 'inline-source-map',
  devServer: {
    contentBase: './dist',
    hot: true,
  }
});

```

# webpack.prod.js
```
const merge = require('webpack-merge');
const common = require('./webpack.common.js');
const path = require('path');

const CleanWebpackPlugin = require('clean-webpack-plugin');
const OptimizeCSSAssetsPlugin = require("optimize-css-assets-webpack-plugin");
const HtmlWebpackPlugin = require('html-webpack-plugin');
const UglifyJsPlugin = require("uglifyjs-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const Purifycss = require('purifycss-webpack')
const glob = require('glob-all')
const copyWebpackPlugin = require("copy-webpack-plugin");

module.exports = merge(common, {
  mode: 'production',
  optimization: {
    minimizer: [
      new UglifyJsPlugin({ //处理js压缩
        cache: true,
        parallel: true,
        sourceMap: true // set to true if you want JS source maps
      }),
      new OptimizeCSSAssetsPlugin({}) //压缩css
    ]
  },
  plugins: [
    new CleanWebpackPlugin(['dist/*']), //去除dist目录
    new MiniCssExtractPlugin({ //提取css
      filename: "css/[name].[contenthash:8].css",
      chunkFilename: "css/[name].[contenthash:8].css"
    }),
    new copyWebpackPlugin([{ //这个可以提取静态文件，以后还会增加
        from: path.resolve(__dirname, "../src/assets"),
        to: './assets',
        ignore: ['.*'] //排除目录
    }]),
    new Purifycss({ //tree shaking css
      paths: glob.sync([
        path.join(__dirname, '../src/*.html'),
        path.join(__dirname, '../src/*.js')
      ])
    }),
  ]
});
```
