---
title: Newproject
date: 2018-09-14 23:43:02
tags: [html5,css3,jquery]
categories: [github项目笔记]
---
# 穆克网七夕 html5 一些记录吧
项目地址：[穆克网七夕](https://github.com/DemoorBug/Newproject)
## HTML5+CSS3+jquer.html
<!-- more -->
*第一天*
:nth-child
```css
案例
p:nth-child(2) {background:#ff0000;} 
 选中P的第二个标签  
 规定属于其父元素的第二个子元素的每个 p 的背景色：
 ```
.find(":first") 
```css
这个很神奇  获取父级的html，但是不获取父级，很奇怪
获取第一个子节点
```
transform: translate3d(-740px, 0px, 0px);
transition-timing-function: linear;
transition-duration: 5000ms;
```css
translate3d(x,y,z)	定义 3D 转换。
transition-timing-function	规定速度效果的速度曲线。
transition-duration	规定完成过渡效果需要多少秒或毫秒。

_transition 的全名写法：_
transition-property	规定设置过渡效果的 CSS 属性的名称。
transition-duration	规定完成过渡效果需要多少秒或毫秒。
transition-timing-function	规定速度效果的速度曲线。
transition-delay	定义过渡效果何时开始。
_transform 的各种用法：_
none	定义不进行转换。	
matrix(n,n,n,n,n,n)	定义 2D 转换，使用六个值的矩阵。	
matrix3d(n,n,n,n,n,n,n,n,n,n,n,n,n,n,n,n)	定义 3D 转换，使用 16 个值的 4x4 矩阵。	
translate(x,y)	定义 2D 转换。	
translate3d(x,y,z)	定义 3D 转换。	
translateX(x)	定义转换，只是用 X 轴的值。	
translateY(y)	定义转换，只是用 Y 轴的值。	
translateZ(z)	定义 3D 转换，只是用 Z 轴的值。	
scale(x,y)	定义 2D 缩放转换。	
scale3d(x,y,z)	定义 3D 缩放转换。	
scaleX(x)	通过设置 X 轴的值来定义缩放转换。	
scaleY(y)	通过设置 Y 轴的值来定义缩放转换。	
scaleZ(z)	通过设置 Z 轴的值来定义 3D 缩放转换。	
rotate(angle)	定义 2D 旋转，在参数中规定角度。	
rotate3d(x,y,z,angle)	定义 3D 旋转。	
rotateX(angle)	定义沿着 X 轴的 3D 旋转。	
rotateY(angle)	定义沿着 Y 轴的 3D 旋转。	
rotateZ(angle)	定义沿着 Z 轴的 3D 旋转。	
skew(x-angle,y-angle)	定义沿着 X 和 Y 轴的 2D 倾斜转换。	
skewX(angle)	定义沿着 X 轴的 2D 倾斜转换。	
skewY(angle)	定义沿着 Y 轴的 2D 倾斜转换。	
perspective(n)	为 3D 转换元素定义透视视图。
```
封装
```css
封装，即隐藏对象的属性和实现细节，仅对外公开接口。
封装的目的是增强安全性和简化编程，使用者不必了解具体的实现细节，而只是要通过外部接口，以特定的访问权限来使用类的成员
这个主题案例，我采用的是面向接口编程的写法，简单的说就是将行为封装分布在各个对象中，并让这些对象自己各自负责自己的行为，这也是面向对象设计的一个优点
就拿页面切换的效果来说，在某一时刻，元素A需要让页面进行切换，那么元素A不需要关心页面是怎么切换的，它只能要调用到一个接口方法能让页面切换就行了
```
@keyframes
animation
```css
/*规定 @keyframes 动画的名称。*/
animation-name: person-slow; 
/*规定动画完成一个周期所花费的秒或毫秒。默认是 0*/
animation-duration: 950ms;
/*规定动画被播放的次数。默认是 1。 infinite(循环播放)*/
animation-iteration-count: infinite;
/*动画切换的方式是一帧一帧的改变*/
animation-timing-function: steps(1, start);
```
animation-play-state: paused;
```css
暂停动画
```
*js的一些技巧*
我发现这些人喜欢这样编码 
```css
var getValue = function(className) {
    var $elem = $('' + className + '');
        // 走路的路线坐标
    return {
        height: $elem.height(),
        top: $elem.position().top
    };
};

// 路的Y轴
var pathY = function() {
    var data = getValue('.a_background_middle');
    return data.top + data.height / 2;
}();
_定义函数的时候 用var getVlaue = function(){}  这样的方式，而且就算是求值也要写到函数里面，get_
<i>哇塞，这好像封装了两个方法，好厉害啊 点赞 这样好像有些不清晰，为了以后方便查找源，这里附上链接</i>
<a>http://www.imooc.com/code/8897</a>

```
判断对象
```css
obj instanceof Object   
_上面的基于原型链判断的_
```
jquery.transit
```css
过度动画 和 animation很像
    $boy.transition({
        'left': $("#content").width() + 'px',
    }, 10000, 'linear', function() {});

```
Promises
```css
dtd.then(function() {
   //操作1
}).then(function() {
   //操作2
}).then(function() {
  //操作3
})
_上面是一个案例模型，下面的才是一个例子_
function animate2() {
    var dtd = $.Deferred(); // 生成Deferred对象
    $("#block4").animate({
        width: "50%"
    }, 2000, function() {
        dtd.resolve(); // 改变Deferred对象的执行状态
    });
    return dtd;
}
_var anim = animate1();  为了让代码清晰_  
 anim.then(function() {
    $("#block3").text('block3动画动画直接结束');
    return animate2();
}).then(function() {
    $("#block4").text('block4动画动画直接结束');
});
```

