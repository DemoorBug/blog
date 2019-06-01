---
title: 移动webapp
date: 2019-04-12 18:47:45
tags: [html5, css3]
categories: 移动开发
---
# 移动webapp开发

<!-- more -->
# 视口
## viewport 标签的介绍
```html
<mate name="viewport" content="width=device-width">
```
width=device-width 视口宽，自适应
```html
<mate name="viewport" content="height">
```
很少用height，基本不用
```html
<mate name="viewport" content="initial-scale=1">
```
initial-scale=1 缩放比
相当于设置width=device-width，但是用的时候都得写，因为个浏览器有bug
```html
<mate name="viewport" content="user-scalable=no">
```
不允许用户缩放
```html
<mate name="viewport" content="maximum-scale=1, minimum-scale=1">
```
最大缩放比，和最小缩放比

标准写法
```html
<meta name="viewport" content="width=device-width, initial-scale=1, user-scalable=no, maximum-scale=1, minimum-scale=1">
````
## 获取视口宽度
```js
console.log(window.innerWidth);
console.log(document.documentElement.clientWidth);
console.log(document.documentElement.getBoundingClientRect().width);
// 兼容个浏览器写法
var width = document.documentElement.clientWidth || window.innerWidth
// dpr
console.log(window.devicePixelRatio);

```

# Flex 布局，弹性布局
这个box就为flex的container容器
```css
.box {
  display: flex | inline-flex
}
```
## flex布局属性
### `flex-direction` 决定主轴方向
`row`: 默认值
`row-reverse`: 主轴为水平方向，起点在右端
`column`: 主轴水平方向变为垂直方向，起点在上
`column-reverse`: 主轴为垂直方向，起点在下

### `flex-wrap` 换行
一条轴线排不下，是否换行
`nowrap`: 不换行，默认值
`wrap`: 换行
`wrap-reverse`: 相反换行
### `flex-flow` 简写，上面两种属性的简写
```css
.box {
  flex-flow: row nowrap; //默认值
}
```
### `justify-content` 属性定义了项目在主轴上的对齐方式   常用
`flex-start`: 默认值
`flex-end`: 右对齐
`center`: 居中
`space-between`: 两端对齐
`space-around`: 每个项目两侧间隔相等
### `align-items` 交叉轴对齐方式
`flex-start`: 不是默认值
`flex-end`: 交叉轴终点对其
`center`: 中点对齐
`baseline`: 项目的第一行文字的基线对其
`stretch`: 默认值，项目未设置高度或设为auto，将沾满整个容器的高度
### `align-content` 多根轴线对齐方式(多行)，如果项目只有一根轴线，则不起作用
属性值和`justify-content`一致, 默认值为`stretch`

## 项目属性
###　`order` 属性定义项目的排列顺序，数值越小，排列越靠前，默认为0
```css
.item-1 {
  order: -1;
}
```
### `flex-grow` 默认值为0，类似于，栅格系统，不过更灵活
```css
.item-1 {
  flex-grow: 1;
}
.item-2 {
  flex-grow: 2;  //2的值就占用项目.item-1的二倍
}
```
如果有固定宽度，则减去这个固定宽度，等分其他空间
如果两个值都为1，则总共有两份，大家都占用了2/1空间，
### `flex-shrink` 默认值为1，属性定位缩小比例，和`flex-grow`是相反的
```css
.item-1 {
  flex-shrink: 0; //不缩放该项目
}
```
### `flex-basis` 默认值为auto,项目占据的主轴空间，如果有width属性，就会覆盖width属性, 浏览器根据这个属性，计算主轴是否有多余空间

### `flex` 是前面3给的简写，不算`order`
```css
.item-1 {
  flex: 1; //相当于flex: 1 1 auto; 相当于 flex: auto
}
```
### `align-self` 默认auto, 表示继承align-items, 可覆盖align-items, 如果没有父元素，则等同于stretch
属性都可以借鉴`align-items`

# 媒体查询

```css
@media screen and (min-width: 900px) {}
```
`screen` 屏幕类型，除了屏幕打印设备和阅读设备

## 默认值
all(default)
screen / print(打印预览，可以做简历) /speech(阅读设备，残障人士使用)
```css
@media all and (min-width=900px) {}
```

`all`因为是默认值，可以不写
```css
@media (min-width=900px) {}
```
## 媒体查询中的逻辑

与(and)
或(,)
非(not)

或写法：
```css
@media screen and (min-width=1024px), (max-width:900px) {}
```
值得注意的是，或(,)之后的算是个体，也就是说，我们使用了默认值all的写法

非写法：
```css
@media not screen and (min-width=1024px), (max-width:900px) {}
```
同理，只是前半段，或之后算一个整体

## 媒体特征表达
`max-width, min-width, width`
设备像素比
dpr
`-webkit-device-pixel-ratio: 2,-webkit-max-device-pixel-ratio: 2,-webkit-min-device-pixel-ratio: 2`

`orientation`
有两个值：`landscape` 横屏(宽比高大)，`portrait` 竖屏
后面还有值，几乎不用，了解即可，这里就没有记录，要看的就是3-19 媒体查询-基础(2).vep

## 媒体查询策略
设置断点
xs: < 576px
sm: 576px ~ 768px
md: 768px ~ 992px
lg: 992px ~ 1200px
xl: > 1200px

```css

@medio screen and (max-width=576px) {}

@medio screen and (min-width=577px) and (max=768px) {}

@medio screen and (min-width=769px) and (max=992px) {}

@medio screen and (min-width=993px) and (max=1200px) {}

@medio screen and (min-width=1201px) {}
```

# 移动端单位问题
## em的技巧
```css
.font {
  font-size: 16px;
  text-indent: 2em; //这个em就会继承当前元素字体的大小，也就是2个字节，很方便啊
}
```
## rem主流
这么写，内容就可以根据宽度自适应变化，666，公式就是：
(当前页面宽度/375) * 20 这个比例，为什么是这个比例呢
```html
  <style type="text/css" media="screen">
    html {
      font-size: 20px;
    }
    .box {
      width: 2.5rem;
      height: 2.5rem;
      background: #222;
    }
  </style>
</head>
<body>
  <div class="box"></div>
  <script type="text/javascript">
    window.onresize = function() {
      document.documentElement.style.fontSize = (document.documentElement.clientWidth / 375)* 20 +'px';
    }

  </script>
```
# 响应开发，因为都懂，就记一些不懂的吧
## img图片的处理
```css
img {
  display: block; /*因为默认是inline-block;*/
  /*但是这样解决会造，一行显示换行问题*/
  /*最优解决*/
  vertical-align: top; /*这个属性居然没用过，而且所有浏览器都支持*/
  width: 100%;
  border: none;
}
```
以上选一即可

# 通用适配
设计稿750px为例
750px 1rem = 750 / 18.75 = 40
js动态去设置font-size
```js
// document.documentElement.getBoundingClientRect().width
// window.innerWidth
(function() {
  'use str'
  setRemUnit()
  window.onresize = setRemUnit;
  // window.addEventListener('resize', setRemUnit); 都可以

  function setRemUnit() {
    var docEl = document.documentElement;
    var ratio = 18.75;
    var viewWidth = docEl.getBoundingClientRect().width || window.innerWidth; //兼容处理
    docEl.style.fontSize = viewWidth / ratio + 'px'
  }

})();

```

# 通用适配解决方案，解决1px边框显示过'粗'问题
```js
(function() {
  'use str';

  // 获取dpr
  var docEl = document.documentElement,
    viewportEl = document.querySelector('meta[name="viewport"]'),
    dpr = window.devicePixelRatio || 1,
    maxWidth = 540,
    minWidth = 320;

  dpr = dpr >= 3 ? 3 : (dpr >= 2 ? 2 : 1);

  docEl.setAttribute('data-dpr', dpr);// 自己设置dpr，后面有用处
  docEl.setAttribute('max-width', maxWidth);
  docEl.setAttribute('min-width', minWidth);


  var scale = 1/dpr,
      content = 'width=device-width, initial-scale='+ scale +', maximum-scale='+ scale +', minimum-scale='+ scale +', user-scalable=no';

  if (viewportEl) {
    viewportEl.setAttribute('content', content)
  } else {
    viewportEl = document.createElement('meta');
    viewportEl.setAttribute('name', 'viewport');
    viewportEl.setAttribute('content', content);
    document.head.appendChild(viewportEl)
  }




  setRemUnit()
  window.onresize = setRemUnit;
  // window.addEventListener('resize', setRemUnit); 都可以

  function setRemUnit() {
    // var docEl = document.documentElement;
    var ratio = 18.75;
    var viewWidth = docEl.getBoundingClientRect().width || window.innerWidth; //兼容处理

    // 设置最大最小值，页面过大后不设置fontSize
    if (maxWidth && (viewWidth / dpr > maxWidth)) {
      viewWidth = maxWidth * dpr;
    } else if (maxWidth && (viewWidth / dpr < minWidth)){
      viewWidth = minWidth * dpr;
    }

    docEl.style.fontSize = viewWidth / ratio + 'px';
  }

})();

```

# 移动端事件

触摸事件，手势事件，传感器事件，主要讲的是触摸事件，其他两个兼容性堪忧，而且手势事件也可以用触摸事件代替，传感器事件就是手机倾斜什么的，但是兼容性不ok

触摸事件可以非为两种：
`touch` 事件，最早出现的，兼容性ok
`pointer` 事件 ，这个是微软出的，比较友好，但是呢实现的厂商不多，兼容性不好，但是很有意义，把鼠标事件和触摸事件统一为指针事件，不论是pc端还是移动端，只用使用`pointer`事件就ok了

## touch 事件
`ontouchstart`  触摸开始执行
`ontouchmove`   移动执行
`ontouchend`    触摸结束执行
`ontouchcancel`  不常用，发生触摸中断的时候执行，当我们点击的时候，突然来电话，界面跳转，称之为触摸中断。系统级事件

`document.ontouchstart = function () {}` 这种写法不推荐，如果要兼容IE8浏览器及一下，还是用这种

`document.addEventListener('touchstart', function(){}, false)` 推荐写法，IE9及以上兼容。第三个参数表示冒泡，true表示捕获

## touch 事件event 对象
```js
document.addEventListener('touchstart', function(ev){
  console.log(ev)
}, false)
```
以上对象属性含义:
`altKey: false` 触摸的时候是否有按住alt键，手机哪来的alt键
以下三个和冒泡有关
`bubbles: true`  目前是否是冒泡
`cacelBubble: false` 目前是否取消冒泡
`cancelable: true` 是否可以取消冒泡
以下就讲了一下主要用到的，其他就不讲了
`type` 知道当前什么事件，和pc端事件一样
`target` 当前元素，什么元素响应你的
`changeTouches` 捕获发生变化的手指，触摸列表，类数组(没有数组的方法),一般情况下都使用这个，其中一个原因是`ontouchend`事件以下两种捕获不到东西
`targetTouches` 捕获到物体上的手指
`touches` 捕获到屏幕上所有手指头

常用`changeTouches`
```js
document.addEventListener('touchstart', function(ev){
  var touch = ev.changeTouches[0] //一般都是单指
}, false)
```
`changeTouches`里面的属性：
`clientX: ` 可视区范围内的坐标
`clientY: ` 可视区范围内的坐标
`pageX` 有滚动条的时候，也会把滚动条的距离算上，而不仅仅是可视区域
`pageY`
`radiusX` 指头的触摸大小半径
`radiusY`

## 这里讲到了拖动，我没有做，明天补上，今晚记一些技巧
chrome的a点击有个高亮，当时自己还是百度了好久，我晕
```css
.a:hover {
  -webkit-tap-highlight-color: transparent;
}
```
还有就是用transform的时候用translate3d，会开启设备的GPU加速
```css
.backtop {
  transform: translate3d(x, y, 0); /*会开启GPU加速*/
}
```
补：
```html

  <style type="text/css" media="screen">
    html {
      height: 2000px;
    }
    .top {
      height: 20px;
      width: 20px;
      position: fixed;
      bottom: 20px;
      right: 20px;
      border-radius: 50%;
      background: rgba(0,0,0,.3);
      /*transition: all .3s;*/
    }
  </style>
</head>
<body>
  <div class="top" id="top">
  </div>
    122

  <script>
    var docEl = document.documentElement;
    var topN = document.getElementById('top');

    var coordinate = {
      pageX: 0,
      pageY: 0
    }
    var page = {
      pageX: 0,
      pageY: 0
    }
    var isTouchMove = false;

    topN.addEventListener('touchstart', function(ev) {
      var changedTouches = ev.changedTouches[0]
      coordinate.pageX = changedTouches.pageX;
      coordinate.pageY = changedTouches.pageY;
    })
    topN.addEventListener('touchmove', function(ev) {
      ev.preventDefault()

      isTouchMove = true;

      var changedTouches = ev.changedTouches[0]
      var x = changedTouches.pageX - coordinate.pageX + page.pageX;
      var y = changedTouches.pageY - coordinate.pageY + page.pageY;

      ev.target.style.transform = 'translate3d('+ x +'px,'+ y +'px,0)'
    })
    topN.addEventListener('touchend', function(ev) {
      if (!isTouchMove) {return};
      console.log('touch')
      var changedTouches = ev.changedTouches[0]
      page.pageX += changedTouches.pageX - coordinate.pageX;
      page.pageY += changedTouches.pageY - coordinate.pageY;

      isTouchMove = false
    })
  </script>
```

# 移动端调试


## Vorlon.js

## 多端同步工具Browsersync

## 终端检测，navigator
最好后端来做，但是前端也是可以做的

```js

var isMobile = navigator.userAgent.match(/android|iphone|ipad|ipod/i)

if (isMobile) {
  location.href = 'https://m.imooc.com'
} else {
  location.href = 'https://www.imooc.com'
}
```
可以根据这段代码，加载各端自己的代码，比如响应式开发

# 移动开发常见问题
解决html5标签兼容问题引入
`html5shiv.js`

## css3 兼容问题
用`modernizr.js` 这个库来兼容，写两套样式

## click 300毫秒延迟解决
原因是手机端的double click双击造成的，
解决办法`fastclick`,很多浏览器已经解决就是给`viewport`添加`width=device-width`
## 文字溢出问题
普通解决方法不能配合`flex` 布局，必须给元素套一层
```html
<style type="text/css" media="screen">
  .text-ellipsis {
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: normal;
  }
  .flex {
    display: flex;
    justify-content: center;
    align-items: center;
  }
</style>

<div class="text-ellipsis flex">
  <span>文字</span> //套一层就好了
</div>
```

多行文字溢出问题解决
```html
<style type="text/css" media="screen">
  .flex {
    display: flex;
    justify-content: center;
    align-items: center;
  }
  .multiline-ellipsis {
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    -webkit-line-clamp: 3; 这个就可以控制显示几行文字
    -webkit-box-orient: vertical;
    white-space: normal !important;
    word-wrap: break-word;
  }
</style>

<div class="multiline-ellipsis flex">
  文字
</div>
```

## javascript兼容
做特性检测，不要做浏览器检测
这个是fastclick.js的特性检测做法
```js
if ('addEventListener' in document) { //这个是fastclick.js的特性检测做法
  document.addEventListener('DOMContentLoaded', function() {
    FastClick.attach(document.body)
  }, false)
};
```
特性检测就很简单，就是判断以下，要加`window`的原因是因为如果没有该元素就会报错，未定义，而挂载到`window`上，他就会报`undefined`
```js
if (requestAnimateionFrom) { //错误写法，如果不存在就会变成一个没有声明的变量

}

if (window.requestAnimateionFrom) { //正确做法

}

```


前缀
```js
var requestAnimationFrame = window.requestAnimateionFrom || window.webkitRequestAnimationFrame || window.mozRequestAnimationFrame || window.msRequestAnimationFrame || window.oRequestAnimationFrame || function(fn) {setTimeout(fn, 16)};
```
## 移动端动画
优先使用 `requestAnimateionFrom`, 请求动画帧
css3使用`transition`, `animation`

`canvas` 要配合`setTimeout`,`setInterval`做动画，不能用css3，而DOM动画，就可以用提到的所有
## 水平居中和垂直居中
```html
水平居中：
`text-align: center` 有固定宽度就可以使用
`margin: 0 auto` 针对块级容器，有固定宽度
`position+margin-left/translate` 以前仅用到了`margin`来解决，不过`translate`可以解决没有固定宽度，也可以实现
`flex`终极解决方案
垂直居中
`line-height` 有固定高度可以使用，仅仅只能用于单行文字，其实配合
`position+margin-top/translate` 同理
`flex`终极解决方案

## Zepto
这里面虽然都很基础，和jQuery都差不多，不过还是能学到东西的，比如`window.onload`和`$(document).ready(function (){})`的区别，前者是页面全部加载完成，包括图片，js，css，dom,后者则是dom加载完毕，肯定是dom加载完毕更高效啊
```

还有事件的命名空间，
```js
$(document).on('click.muke', function (e) {
  console.log('click');
  $(this).off('.muke') //大概是这么写的？
})
```
这样就仅仅会取消一个，以前老师教的是创建一个变量
```js
var ev;
$(document).on('click', ev = function (e) {
  console.log('click');
  $(this).off(ev)
})
```

## 按需加载，我觉得还是另一个老师讲的细，这里就不深究了，看那个老师的就ok，这个参考一下就好

# 实战开始,这里就直接上vue项目了
页面最大宽度640，最小宽度320
设计稿为 375px 的页面，扩大两倍到750px，做标注，同时等比放大1.5倍生成宽度位1125的设计稿，在这里面切图

项目初始化完毕了，很简单，懒加载，重定向什么的

全局的样式都写在`assets/less`目录，这样便于管理，而且可以全局引入，其他页面都不用引入

**固定定位，以前的手机兼容不好，最好使用绝对定位来模拟**

## 目录结构
src/
-- assets 全局文件
-- base 基础组件
-- components 公共组件
-- views 所有页面放的位置，一个文件夹代表一块，index为默认出口
-- App.vue
-- main.js
-- router.js
-- store.js
**基础组件和业务逻辑毫无瓜葛**
也就是说base里面的组件都可以扩展，slot插槽


## 用到的插件统计
`fastclick` 解决移动端300点击延迟
`Vue-Awesome-Swiper` vue 封装的 swiper插件，轮播
`jsonp` 获取jsonp数据
`vue-lazyload` 延迟加载


## 开发vue项目用到的技巧
header 部分公共化
使用slot插槽，$slots判断是否使用了该插槽，没使用就隐藏
这里遇到个问题，定位问题如果前面元素不显示就会导致，最后一个元素无法右对齐，所以呢，有两种方案，第一种就是把第一个元素不用v-if判断，一直显示，另一种是用css3选择器完成，不过我觉得没必要，用到的选择器~ , 不过呢，我觉得这种方法很笨，不会用
```html
<!-- navbar.vue -->
<div class="mine-navbar">
  <div class="mine-navbar-left" v-if="$slots.left">
    <slot name="left"></slot>
  </div>
  <div class="mine-navbar-center" v-if="$slots.center">
    <slot name="center"></slot>
  </div>
  <div class="mine-navbar-right" v-if="$slots.right">
    <slot name="right"></slot>
  </div>
  <h1 class="mine-navbar-title" v-if="title">
    <span class="mine-navbar-text ellipsis" v-text="title"></span>
  </h1>
</div>

<style lang="less" scoped>
@import '~assets/less/_mixins.less';

.mine-navbar {
  display: flex;
  justify-content: space-between;
  align-items: center;
  height: @navbar-height;
  background-color: #fff;
  &-left {
    margin-left: 20/@r;
    ~ .mine-navbar-right {  //新增代码
      position: static;
    }
  }
  &-center {
    flex: 1;
    margin: 0 20/@r;

    ~ .mine-navbar-right {  //新增代码
      position: static;
    }
  }
  &-right {
    position: absolute;   //新增代码
    right: 0;
    display: flex;    //用了绝对定位父元素的flex就会失效，所以要搞这个
    justify-content: center;
    align-items: center;
    height: 100%;

    margin-right: 20/@r;
  }
  &-title {
    position: absolute;
    left: 20%;
    right: 20%;
    text-align: center;
    display: flex;
    align-items: center;
  }
  &-text {
    .ellipsis();
    font-size: 18px;
    width: 100%;
    line-height: 1.5;

  }
}

</style>
```
使用插槽

```html
<template>
  <!-- 差点忘记了，这里不能用:title.加：后面调用的就是表达式了 -->
  <me-navbar title="哈哈哈">
    <i class="iconfont" slot="left"></i>
    <div slot="center"></div>
    <i class="iconfont" slot="right"></i>
  </me-navbar>
</template>

<script>
import MeNavbar from 'navbar.vue'

export defult {
  name: 'homeHeader',
  components: {
    MeNavbar
  }
}

</script>
```
### props 接收效验
不过我觉得把，我自己写好像没必要效验，又不是团队开发，就算是团队开发，这个东西不给别人用就没必要效验？估计是我现在还没有接触到，所以还是写一下吧，逼格？嗯，目前为止
```js
props: {
  direction: {
    type: String,
    default: 'horizontal',
    validator (value) { // value就可以接收到传入的参数
      // 如果返回true代表成功效验，flase失败
      return [
      'horizontal',
      'vertical'
      ].includes(value)
    }
  }
}
```
