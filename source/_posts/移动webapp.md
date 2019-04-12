---
title: 移动webapp
date: 2019-04-12 18:47:45
tags: [html5, css3]
categories: [移动开发]
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
@media screen and (min-width: 900px)
```
`screen` 屏幕类型，除了屏幕打印设备和阅读设备

## 默认值
all(default)
screen / print(打印预览，可以做简历) /speech(阅读设备，残障人士使用)
```css
@media all and (min-width=900px)
```

`all`因为是默认值，可以不写
```css
@media (min-width=900px)
```
## 媒体查询中的逻辑

与(and)
或(,)
非(not)

或写法：
```css
@media screen and (min-width=1024px), (max-width:900px)
```
值得注意的是，或(,)之后的算是个体，也就是说，我们使用了默认值all的写法

非写法：
```css
@media not screen and (min-width=1024px), (max-width:900px)
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

@medio screen and (max-width=576px)

@medio screen and (min-width=577px) and (max=768px)

@medio screen and (min-width=769px) and (max=992px)

@medio screen and (min-width=993px) and (max=1200px)

@medio screen and (min-width=1201px)
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