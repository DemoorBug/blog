---
title: webpack 4配置
date: 2019-03-08 22:45:23
tags:
categories:
---

# webpack.common.js
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