---
title: comprehensive
date: 2018-09-14 20:06:16
tags: [php,jquery,css3,html5,canvas,mongodb,node,mysql]
categories: [github项目笔记,笔记MAX]
---

## 以前做过的东西，总结一下
几乎涵盖了16年学的所有东西

## CSS3+JS 实现超炫的散列画廊特效
项目地址：[CSS3+JS 实现超炫的散列画廊特效](https://github.com/DemoorBug/comprehensive/tree/master/CSS3%2BJS%20%E5%AE%9E%E7%8E%B0%E8%B6%85%E7%82%AB%E7%9A%84%E6%95%A3%E5%88%97%E7%94%BB%E5%BB%8A%E7%89%B9%E6%95%88)
<!-- more -->
```css
**3D效果编写**
2016.4.25
transition：property duration timing-function delay
property 规定设置过渡效果的css属性的名称
duration 规定完成过渡效果多少秒或毫秒
timing-function  规定速度效果的速度曲线，默认ease
dalay 定义过渡效果延迟多久开始，默认0
**知识**
-webkit-font-smoothing:antialiased;用于字体平滑
< body onselectstart="return false">
# onselectstart="return false"  防止文字被选中 
-webkit-perspective:800px; # 支持3D
box-sizing: border-box;    # 内容和padd都会在border边框之内去呈现
-webkit-transform-style:preserve-3d;    #支持子元素的3D效果
-webkit-transform:translate(0px,0px) rotateY(0deg);		 			#定义位移以及沿着Y轴旋转
-webkit-backface-visibility:hidden;    当元素不面向屏幕时隐藏
```

## WEB APP
项目地址: [WEB APP](https://github.com/DemoorBug/comprehensive/blob/master/WEB%20APP/web%20App.html)

**Touch基础事件**5.30
touchstart:手指触摸屏幕触发(已经有手指放屏幕上不会触发)
touchmove:手指在屏幕滑动,连续触发
touchend:手指离开屏幕时触发
touchcancel:系统取消touch时候触发(不常用 )
_android的4.4,4.0的bug：支触发touchstart，和一次的touchmove不触发_
_加入event.preventDefault_
*除常见的事件属性外，触摸事件包含专有的触摸属性*
touches:跟踪触摸操作的touch对象数组
targetTouches:特定事件目标的touch对象数组
changaoTouches:上次触摸改变的touch对象数组
*每个touch对象包含属性:*
clientX:触摸目标在视口中的x坐标
clientY:触摸目标在视口的y坐标
identifier:标识触摸的唯一ID。
pageX:触摸目标在页面中的x坐标(包含滚动)
pageY:触摸目标在页面中的y坐标(包含滚动)
screenX:触摸目标在屏幕中的x坐标
screenY:触摸目标在屏幕中的y坐标
target:触摸的DOM节点目标。
**移动框架库**5.25
*Tab自定义事件*
_Fastclick库_
zepto.js
_tab点透bug_
**viewport 视图**5.20
_980px(ios)_
_< meta name="viewport" content="name=value,name=value>_
width: 设置布局viewport的特定值('device-width')
initial-scale: 设置页面的初始缩放 1
minimum-sacle: 最小缩放
maximum-scale: 最大缩放
user-scalable: 用户能否缩放
_垂直居中  CSS3_
.myoff-wrapper {
	position:absolute;
	top:50%;
	left:50%;
	z-index:3;
	-webkit-transform:translate(-50%,-50%);
	border-radius:6px;
	background:#FFF
}
**基本知识-像素**5.19
*移动开发像素知识*
_px: css pixels 逻辑像素，浏览器使用的抽象单位_     1px  == 1像素
_dp,pt: device independent pixels 设备无关像素_     1dp  == 4/1像素  一个像素就会有4个物理像素(dp)渲染
_dpr: devicePixelRatio 设备像素缩放比_
_计算公式：1px = (dpr)_2_ * dp_
*iphone5是320px*568px? -> 因为 dpr = 2*
平面上：1px = (2)_2_ * dp         
纬度上：1px = 2 * dp
因此换算公式
640dp*1136dp == 320px*568px
_DPI:打印机每英寸可以喷的墨汁点(印刷行业)_
_PPI:屏幕每英寸的像素数量，既单位英寸内的像素密度_
目前。在计算机显示设备参数描述上，二者意思表达的是一样的。

计算公式： 以iPhone5为例子
ppi= √ (1136_2_ * 640_2_) /4 = 326ppi (视网膜Retina屏)
*PPI越高，像素数越高，图像越清晰*
当可视度越低(小)，系统默认设置缩放越大
				ldpi			mdpi			hdpi			xhdpi
ppi             120             160 			240				320
默认缩放比		0.75   			1.0				1.5 			2.0
_1px = dpr_2_ * dp_

## canvas
项目链接：[canvas](https://github.com/DemoorBug/comprehensive/tree/master/canvas)

**这几天看css3去了**4.27

**第一天**4.19	
canvas 指定大小最好在  width="" height="" 最好在标签能直接加上css指定的实际是canvas显示的大小 
```js
_var canvas = document.getElementById('canvas');
var context = canvas.getContext('2d');   #
使用context进行绘制_
可以判断 canvas.getContext('2d');   #是否为空  
*
	context.moveTo(100,100); 画一条直线开始
	context.lineTo(700,700);   结束
	context.lineWidth = 5; 线条宽
	context.strokeStyle = "#005588"
	context.stroke();
*
_ 绘制一个三角并填充
	context.moveTo(100,100); 
	context.lineTo(700,700);
	context.lineTo(100,700);
	context.lineTo(100,100);
	// context.strokeStyle = "#005588";
	context.fillStyle = "rgb(2,100,3)"
	context.fill();
_
```

## css3 重要的
项目地址：[css3](https://github.com/DemoorBug/comprehensive/tree/master/css3)

**置顶**  19.48  感冒头疼  特此请假  2016-5-18  —— 2016-5-19
**tap事件基础**5.24
自定义事件
_自定义Tap事件原理：_
在touchstart、touchend时记录时间、手指位置，在touchend时进行比较，如果手指位置为同一位置(或允许移动一个非常小的位移值)且时间间隔较短(一般认为200ms)，且过程中未曾触发过touchmove，即可认为触发了手持设备上的“click”，一般称他为“tap”
**样式处理**5.24
在移动web页面上渲染图片，为了避免图片产生模糊，图片的宽度用物理像素单位渲染，即100*100的图片，应该使用100dp*100dp
width: (w_value/dpr)px;
height: (h_value/dpr)px;
_边框1px使用2dp渲染_
.sidebar .folder li {
	<!-- border-bottom: 0.5px solid #DDD -->
	padding:8px 0 8px 15px;
	color:#7c7c7c;
	cursor:pointer;
	position:relative;
}
.folder li + li:before {
	position:absolute;
	top: -1px;
	left: 0 ;
	content: '';
	width:100%;
	height:1px;
	border-top:1px solid #DDD;
	*-webkit-transform: scaleY(0.5)*
}
上面这个方案比较成熟的
_相对单位_
em : 是根据父节点font-size为相对单位
rem : 是根据html的font-size为相对单位

em在多层嵌套下，变得非常难以维护
rem更加能成为全局统一设置的度量
_实例rem_
rem基值设置多少好？
回归我们的目的：为了适应手机的屏幕
rem = screen.width / 20;
_——————————————_
默认
html {font-size:32px}
iphone 6
设备的宽度最少为375的时候
@media (min-device-width: 375px) {
	html { font-size:37.5px}
}
iphone 6 plus
@media (min-device-width: 414px) {
	html { font-size:41.4px;}
}
_换算公式_
如iphone5上的rem基值为32px，渲染一张64*64px的div，则用2rem*2rem去渲染。换算公式非常简单：
width：px/rem基值
height：px/rem基值
*多行文本溢出*
//单行文本溢出...
.inaline {
	overflow: hidden;
	white-space: nowrap;
	text-overflow: ellipsis;
}
//多行文本溢出...
.intwoline {
	display: -webkit-box ! important;
	overflow: hidden;

	text-overflow: ellipsis;
	word-break: break-all;

	-webkit-box-orient: vertical;
	-webkit-line-clamp: 2;
}
**媒体查询**5.23
_@media screen and (max-width:1024px)_
*常用媒体查询参数：*
width	—— 视口宽高
height —— 
device-width	设备的宽度
device-height	设备的高度
orientation： 检测设备处于横向
(landscape)还是竖屏 (portrait)
*媒体类型*
screen(屏幕)
print(打印机)
handheld(手持设备)
all(通用)
**flex 属性 代替 弹性盒模型章节**5.20
justify-content: center; /*子元素水平居中*/
align-items: center; /*子元素垂直居中*/
display: -webkit-flex;
_flex-direction:row; /*default*/_  方向  默认
:row-reverse; /*default*/    反方向
:column;  纵向
:column-reverse;
_flex-wrap:nowrap;_ 默认
flex-wrap:nowrap;              }
display:-webkit-flex;          <  这个例子父容器虽然是400  里面内容撑出也不换行  
width:400px; 				   }

:wrap 正常
:wrap-reverse 反方向
_flex-flow:flex-direction flex-wrap;_    z自己体会  上面的结合
_justify-content:flex-start;_  效果通 float：left
:flex-end 	效果同 float: right
:center   水平居中
:space-between	两边对其
:space-around   按间距去划分
_align-self_ 		*父元素用*
:flex-start;   头部
:flex-end;	底部
:center; 	居中
:auto; 	默认
:baseline  字体什么位置
:stretch;   填充满？
_align-items:_      *子元素用*
:flex-start     全部上
:flex-end		下
:center			居中
:baseline       
:stretch
_align-content_
:flex-start   不会像 align-items 切分高度    
:flex-end 	  
:center
:space-between  上下两边
:space-around   等比划分
:stretch		默认  填充好整个容器
**addEventListener 事件**5.19
function addEnd (obj,fn) {
	obj.addEventListener('webkitAnimationEnd',fn,false);
}
document.getElementById('box').onclick = function()
{
	this.className = 'box move';
	addEnd(this,fn)
}
function fn () {
	alert( 'e' )
}
**@-webkit-keyframes move 动画**5.18
_animation:move 5s 1s ease-in 5 _
_animation:2s move_
_必要属性_
	-animation-name 				动画名称(关键帧名称)
	-animation-duration 			动画持续时间
例子
	animation-play-state 播放状态 (running 播放 和 paused 暂停)
_可选属性_
animation-timing-function
	-linear 		均速
	-ease			缓冲
	-ease-in		由慢到快
	-ease-out		由快到慢
	-ease-in-out	由慢到快再到慢
	-cubic-bezier (1,1,1,1)  [0-1]
_可选属性2_
animation-delay			动画延迟
	-只是第一次
animation-iteration-count	重复次数
	-infinite 为无限次
animation-direction 		播放前重置
	-动画是否重置后再开始播放
	-alternate	动画直接从上一次停止的位置开始执行
	-normal		动画第二次直接跳到0%的状态开始执行
**css3.0-3d变换-1**5.17
_transform-style(preserve-3d)_建立3D空间
_perspective:100px_景深
_perspective-origin_景深基点
_backface-visibility_隐藏背面
_Transform_ 新增函数
	·rotateX()
	·rotateY()
	·rotateZ()
	·translateZ()
	·scaleZ()
**matrix(a,b,c,d,e,f)矩阵**5.17
filter:progid:DXImageTransform.Microsoft.Matrix(M11=1,M12=0,M21=0,M22=1,SizingMethod='auto expand');
m11 == a , m12 == c, m21 == b , m22 == d
**transform**5.17
*后写的先执行*
transform:rotate(20deg) skewX(20deg) scaleX(20px);
	·rotate() 旋转函数 取值度数
		-deg 度数
		-Transform-origin 旋转的基点   *放到未变化元素上  最多估计是两个参数*
		*rotateX rotateY rotateZ* 还有z轴
	·skew()倾斜度数 取值度数
		-skewX()
		-skewY()
		transform:skewX(**deg)
		transform:skew(**deg,**deg)
	·scale()缩放函数 取值 正数、小数
		-scaleX()
		-scaleY()
	·translate()位移函数
		-translateX()
		-translateY()
		transform:translateX(**px)
		transform:translate(**px,**px)
_这里有个钟表 以后可以复习下 *妙味云课堂-钟表效果-4*_
_扇形导航 *妙味云课堂-扇形导航* 赶紧学点不知道的 这些案例可以闲下来弄_
**transitionend事件**5.16
_
function addEnd(obj,fn) {
	obj.addEventListener('WebkitTransitionEnd',fn,false);
	obj.addEventListener('transitionend',fn,false);
}
function removeEnd(obj,fn) {
	obj.removeEventListener('WebkitTransitionEnd',fn,false);
	obj.removeEventListener('transitionend',fn,false);
}
addEnd(oBox,function(){      美执行完一个transition就会触发  这个函数   oBox代表当前有tran元素
	this.style.width = this.offsetWidth+100+"px"
	removeEnd(this,addEnd)
})
oBox.onclick = function (){

	
}
_
**transition过渡**5.16
属性
ease： (逐渐变慢)默认值
linear： 匀速
ease-in：加速
ease-out：减速
ease-in-out：先加速后减速
cubic-bezier 贝塞尔曲线(x1,y1,x2,y2)
**新增ui样式3**5.15
*径向渐变*
_radial-gradient:[起点|| left(数值) || 大小 || 形状]<点>,<点>_
background:-webkit-radial-gradient(farthest-corner,red,blue)
形状：ellipse椭圆、circle圆
大小：具体数值或百分比，也可以是关键字
(最近端closest-side，
最近角closest-corner，
最远端farthest-side，
最远角farthest-corner，
包含contain或覆盖cover)
(closest-side,closest-corner,farthest-side,farthest-corner,contain or cover);
	-注意firefox目前只支持关键字
*背景尺寸*
_background-size:x y _
	-background-size:100% 100%;
	-Cover 放大
	-Contain 缩小
_background-origin: border|padding|content_
	·border-box: 从border区域开始显示背景。
	·padding-box: 从padding区域开始显示背景。
	·content-box: 从content区域开始显示背景
_background-clip_ 背景裁切   *这里用到一个ip开机解锁哪里的一个动画，很简单不会的话可以到本节28分钟*
	·border: 从border区域向外裁剪背景
	·padding: 从padding区域向外裁剪背景
	·content: 从content(内容)区域向外裁剪背景
	·no-clip: 从border区域向外裁剪背景
	·text : 从text区域向外裁剪背景 
_遮罩_  就是添加一个png图片作为遮罩
Mask-image
Mask-position
Mask-repeat

**新增ui样式2**5.15
*边框颜色*
_border-colors:red bule syellow_ 多种边框颜色 兼容不是很好
*线性渐变*
属于background-image:的
_linear-gradient([<起点>||<角度>,]?<点>,<点>...);_
linear-gradient(30deg,blue 20px,red 50%);
只能用在背景上
	-IE
	filer:progid:DXImageTransform.Microsoft.gradient(startColorstr='#FFFFFF',endColorstr='#ff0000',GradientType='1');
参数
	-起点：从什么方向开始渐变  默认top
	top right bottom left
	-角度：从什么角度开始渐变
			xxx deg的形式
	-点：渐变点的颜色和位置
			black 50%，位置可选  *50%代表范围吧*
_repeating-linear-gradient(60deg,red 0,blue 30px)  平铺_
**新增ui样式1  box-radius 还不是很熟悉 今天就到这里5.13 **
_box-radius_
100px/200px  前边x轴半径  后边y轴半径
**响应式布局**
_< link rel="stylesheet" type="text/css" href="css.css" media="srceen and (min-width:800px)">_
_< link rel="stylesheet" type="text/css" href="css.css" media="all and (orientation:portrait)">_ 竖屏
_< link rel="stylesheet" type="text/css" href="css.css" media="all and (orientation:landscape)">_ 横屏

_border-img:url 0 10_   0代表上下  10代表左右
 ·border-image-sourceg  引用图片
 ·border-image-slice 切割图片  单位没有*px*
 ·border-image-width 边框宽度
 ·border-image-repeat 图片的排序方式
 	- round平铺，repeat重复，stretch 拉伸
border-left-width:10px;


_style 里面写  _
@media screen and (min-width:400px) and (max-width:500px) {}

@import url('indexc.css') srceen and (min-width:400px) and (max-width:800px);
**分栏布局**
column-width 栏目宽度
column-count 栏目列数
column-gap 栏目距离
column-rule 栏目间隔线
**盒阴影**
_box-shadow: [inset] x y blur [spread] color_
inset 内阴影
blue 模糊半径
spread 扩展阴影半径  先扩展原有形状，再开始画阴影
_box-reflect:方向 距离 渐变(可选)_倒影
direction 方向   above|below|left|right
距离  
渐变(可选)  _-linear-gradient(left,red 0,blue 100%)  线性渐变_
_resize_  自由缩放   *添加后盒子就可以自由伸缩*
	·Both 水平垂直都可以缩放
	·Horizontal 只有水平方向可以缩放
	·Vertical 只有垂直方向可以缩放
	注意：一定要配合overflow：auto一块使用 只有水平方向可以伸缩
_box-sizing:[box]_
content-box    标准盒模型
width/height = border+padding+content
border-box    怪异
width/height = content
**弹性盒模型** 放到父级使用
display:box || inline-box; _display:-webkit-box 老的写法，而且兼容性不够好，需要用前缀。_    *慕课网看到的：-webkit-flex 父级设置  子元素fiex：1*
_Box-orient_定义盒模型的布局方向
	·Horizontal 水平显示
	·vertical 垂直显示
_Box-direction_ 元素排列顺序
	·Normal 正序
	·Reverse 反序
_Box-ordinal-group_ 设置元素的位置
_box-flex_  定义弹性空间
	公式 子元素尺寸=盒尺寸*子元素的box-flex属性值/所有子元素的box-flex属性值的和
_box-pack_ 对盒子富裕的空间进行管理
	·Star 所有子元素在盒子左侧显示，富裕空间在右侧
	·End 所有子元素在盒子右侧显示，富裕空间在左侧
	·Center 所有子元素居中
	·Justify 富裕空间在子元素之间平均分布
_box-align_ 在垂直方向上对元素的位置进行管理
	·Star 所有元素在顶
	·End 所有子元素在底
	·Center 所有子元素居中
**文本属性**
text-shadow x y mohu color   文字阴影
_text-stroke_ x color    描边
_Direction_  定义文字排序方式(全兼容)  *兼容IE6*
Rtl  从右向左排序
Ltr  从左向右排序
注意要配合unicode-bidi 一起使用
_Text-overflow_     两个值 clip  无省略号   Ellipsis 省略号
white-space:nowrap;  不要超出换行
overflow:hidden; 
Text-overflow:Ellipsis
*自定义字体*转换文字格式生成兼容代码 http://www.fontsquirrel.com/fontface/generator
@font-face {
    font-family: 'FontName';
    src: url('FileName.eot');
    src: url('FileName.eot?#iefix') format('embedded-opentype'),
         url('FileName.woff') format('woff'),
         url('FileName.ttf') format('truetype'),
         url('FileName.svg#FontName') format('svg');
    font-style: normal;
    font-weight: normal;
}
b
**伪类选择**
div:target {display: block;background: red;}
_target_ 控制哈希值  锚机那个东西
*控制表单很有用*
_enabled_  可以点击状态表单控件
_disabled_  不可编辑状态
_checked_  控制 type=checkbox || radio 的选中样式   
_给radio用相同name 就可以成为单选框_
*文本新增伪类*
_E:first-line_  文本第一行
_E:first-letter_  第一个文字
*伪元素*
_E::selection_ 表示E元素在用户选中文字时       选中文字的时候样式 这个牛
_E::before_  生成元素之前
_E::after_  生成元素之后
::after {content:"苗圩"; display:block;border:1px solid red;}
_E:not(.h2)_  排除某一项
**选择器**
*属性选择器*
p[属性名='属性值']
p[miaov~='ds']{background: red;}
p[attr~='value']  词组中有value
p[attr^='value']  value开头
p[attr$='value']  value结尾
p[attr*='value']  只要包含了value
p[attr|='value']  横岗开始  或者单纯value    b-leo
*结构选择器*
p:nth-child(1){bakcground: red;}  表示P父元素中的第几个节点
p:nth-child(odd) {}  奇数行
p:nth-child(even) {}  偶数行
p:nth-child(n) {}  n代表从1到正无穷大一个整数  2n  2*0开始到2*正无穷大

_nth-last-child()_从后往前数
p:nth-of-type(1){background: red;}        这个的话 值查找P父级元素下的 P  也就是整合到一个集合 其他东西不管
_p:first-child_ 也就是直接第一个 
_p:last-child_ 最后一个  
_p:first-of-type_ 第一个   元素的第一个节点必须是P
_p:last-of-type_ 最后一个

**
p:empty{background:red;}  是否为空标签
p:only-of-type 表示p元素中只有一个子节点，且这个唯一的子节点类型必须是p。注意：子节点不包含文本节点
_这样听就很难理解   我的理解就是 一个父元素下不能有一样的节点 p *:only-of-type_
p:only-child  表示p元素只有一个子节点。注意子节点不包含文本节点


## html5 重要的
[html5](https://github.com/DemoorBug/comprehensive/tree/master/html5)



clearInterval() setInterval()
**canvas2**5.03
**canvas2**5.03
*绘制圆*
arc(x,y,半径,起始弧度,结束弧度,旋转弧度)
	-x,y:起始位置
	-弧度与角度的关系：弧度=角度*Math.PI/180
	-旋转方向：顺时针(false) 逆时针(true)
	-例子：canvas时钟.html
*绘制其他曲线*
arcTo(x1,y1,x2,y2)
	-第一组坐标、第二组坐标、半径
_例
context.beginPath();
context.moveTo(100,200);
context.arcTo(100,50,200,150,50);
context.stroke();
_
quadraticCurveTo(dx,dy,x1,y1)
	-贝塞尔曲线：第一组控制点、第二组结束点
_例   虽然例子要不了这么多 但是自己玩玩
context.beginPath();
context.moveTo(100,200);
context.quadraticCurveTo(100,100,200,100);
context.stroke();

context.beginPath();
context.moveTo(100,200);
context.quadraticCurveTo(200,200,200,100);
context.stroke();

context.beginPath();
context.moveTo(100,200);
context.lineTo(200,100);
context.stroke();
_
bezierCurveTo(dx1,dy1,dx2,dy2,x1,y1)
	-贝塞尔曲线：第一控制点、第二控制点、第三组结束坐标
_例
context.beginPath();
context.moveTo(100,200);
context.bezierCurveTo(100,100,200,200,200,100);
context.stroke();
_
**canvas 分开写 不然看着木乱**5.03
*边界绘制*
lineJoin:边界点连接点样式
		-miter(默认)、round(圆角)、bevel(斜角)
_例：
context.lineJoin = 'round';
_
lineCap :端点样式  一条直线的两个端点  
		-butt(默认)、round(圆角)、square(高度多出为宽一半的值)
_例：绘制直线后才可
context.lineCap = 'round'
context.lineCap = 'square' 当前宽度一半
_
*绘制路径* 类似ps钢笔
beginPath：开始绘制路径
_例：
context.beginPath()
_
closePath：结束绘制路径 这个是闭合啊
_例：
context.closePath();  这个是闭合啊
_
moveTo:移动到绘制的新目标点
_例：
context.moveTo(100,100);
_
lineTo：新的目标点
_例：
context.lineTo(100,100);
_
*绘制路径—2*
stroke()：画线，默认黑色
fill()：填充，默认黑色
rect():矩形区域
clearRect()：删除一个画布的矩形区域
_例
context.clearRect(0,0,oc.width,oc.height);
_
save()：保存路径
restore()：恢复路径
_例
context.save();

	context.fillStyle = 'red';
	context.beginPath();/*开始*/

	context.rect(20.5,20.5,100,100);
	context.lineWidth = 1;

	context.fill()
	
context.restore()

context.beginPath();/*不会受到影响*/

context.rect(20.5,140.5,100,100);
context.lineWidth = 1;

context.fill()
_

* 直接可以用鼠标进行绘画案例  挺简单
var oc = document.getElementById('cOne');
	context = oc.getContext('2d');  /*目前支持2d  还有个webgl：制作３ｄ绘图　兼容很大问题*/
	oc.onmousedown = function(ev){
		var ev = ev || window.event
		context.beginPath();
		context.moveTo(ev.clientX-oc.offsetLeft,ev.clientY-oc.offsetTop);
		document.onmousemove = function(ev){
			console.log(ev.clientX-oc.offsetLeft,ev.clientY-oc.offsetTop)
			var ev = ev || window.event;
			context.lineTo(ev.clientX-oc.offsetLeft,ev.clientY-oc.offsetTop);
			context.stroke();
		}
	}
		document.onmouseup = function(){
			document.onmousemove = null;
			document.onmouseUp = null;
		}	
*
_方块运动
var oc = document.getElementById('cOne');
context = oc.getContext('2d');  /*目前支持2d  还有个webgl：制作３ｄ绘图　兼容很大问题*/
var num = 0;
context.fillRect(0,0,100,100);
setInterval(function(){
	num++;
	context.clearRect(0,0,oc.width,oc.height);
	context.fillRect(num,num,100,100);
},30)
_
**canvas**5.03
默认大小  300*150px　
创建好getContext('2d')对象后就可以来绘图了
*绘制方块 分为了两个方法*  和边框 写的先后顺序不同  则先覆盖后
fillRect(L,T,W,H) 默认的颜色是黑色
_例：
context.fillRect(50,50,100,100)
_
strokeRect(L,T,W,H) 带边框的方块
				-默认一像素黑色边框，显示出来的不一样原因
_例：
context.strokeRect(50,50,100,100)  这样的话实际为2像素
context.strokeRect(50.5,50.5,100,100)  这样的话实际为2像素
_
*设置绘图*
fillStyle： 填充颜色(绘制canvas是有顺序的)
_例：可以影响两个 要是只想用到一个什么必须用到save 保存路径 restrore 恢复路径
context.fillStyle = 'red';
_
lineWidth： 线绘制，是一个数值
_例：
context.lineWidth = 10;
_
strokeStyle： 边线绘制  颜色
_例：
context.strokeStyle = 'blue';
_

**火狐下无法拖拽**
 			必须设置dataTransfer对象的setData方法才可以拖拽除图片外的其他标签
 *dataTranSfer*出现在event对象下的
 setData();设置数据key和value(必须是字符串)
 getData();获取数据，根据key值，获取对应的value
 _ev.dataTransfer.setData('name','hello')设置键值对_
 *ev.dataTransfer.getData('name')获取设置好的name值*
 _配合拖拽可以完成一个删除功能_
 *dataTransfer对象属性*
 effectAllowed
 		-effectAllowed：设置光标样式(none,copy,copyLink,copyMove,link,linkMove,move,all和uninitialized)
 		_光标样式属性  里面值 各代表不同样式_
 setDragImage() 
 		- 三个参数：指定元素，X ，Y
 		*改变当前拖拽元素*
 files  
 		- 获取外部拖拽的文件，返回一个filesList列表
 		- filesList下有个type属性，返回文件的类型
**讲到了拖拽事件 很实用啊  用到了可以复习**5.02
*拖放操作1*
**历史管理**4.22
onhashchange : 改变hash值来管理
history ：
	-服务器下运行
	-pushState ：三个参数：数据 标题(都没实现) 地址(可选)
	-popstate事件 ： 读取数据 event.state
	-注意： 网址是虚假的，需要服务器指定对应页面，不然刷新找不到页面
**延迟加载JS**4.22
JS的加载会影响后面的内容加载
		--defer   延迟到onload前执行  内联无意义
		--async  异步加载 加载完就触发 有顺序问题
Labjs  异步加载js库  
**data 自定义数据**4.22
dataset
	- data-name: dataset.name
	- data-name-first: dataset.nameFirst
data数据在jquery mobile 在有重要作用
*
	如果以前想获取一组自定义属性  小心的使用getAttribute
*
**JSON的新方法**4.22
parse() : 吧字符串串成json          *只能解释json形式的字符串变为js*
	- 字符串中的属性要严格的加上引号
_
var str = '{ "name" : "hello" }'
var json = JSON.parse(str);
alert( json.name )
_     有兼容问题  要去 json官网 下载 json2.js

stringify() : 吧json转化成字符串
	- 会自动的把双引号加上
*
var str = '{ name : "hello" }'
var json = JSON.stringify(str);
alert( json )
*
新方法与eval的区别
*妙味搜索  对象引用*
_eval_原来是将字符串转换为 js语句的
**html5新的选择器**4.22
*新的选择器*
querySelector            //类似于jq的  $
querySelectorAll
getElementsByClassName
_获取class列表属性_
classList         获得class 集合
	- length : class的长度    *这都是上面的方法* 
	- add() : 添加class方法
	- remove() : 删除class方法
	- toggle() : 切换class方法
**html5表单验证反馈**4.21
validity 对象  ，通过下面的valid可以查看验证是否通过，如果八种验证都通过返回true，一种验证失败返回false
- oText.addEventListener("invalid",fn1,false);
- ev.preventDefault()       阻止默认事件
- valueMissing : 输入值为空时       输入值为空返回true
- typeMismatch: 控件值与预期类型不匹配       输入类型和要求类型不一致返回true
- patternMismatch : 输入值不满pattern正则      输入值是否满足正则要求不一致 返回true
- tooLong : 超过maxLnegth最大限制                  
- rangeUnderflow : 验证的range最小值
- stepMismatch: 验证range的当前值是否符合min、max及step的规则
- customError 不符合自定义验证
	 	》setCustomValidity() ; 自定义验证
*formnovalidate  直接取消表单验证*
_
	var oText = document.getElementById('name');
	Otext.addEventListener('invalid',fn1,false);
	function fn1(){
		alert(this.validity.valid)  当前事件是否通过
		 ev.preventDefault()       阻止默认事件
	}
_
*
	var oText = document.getElementById('name');
	Otext.addEventListener('invalid',fn1,false);
	oText.oninput = function (){
		if(this.value == '1') {
			this.setCustomValidity('请不要输入敏感词')
 		}
	}
	function fn1(){
		alert(this.validity.valid)  当前事件是否通过
		 ev.preventDefault()       阻止默认事件
	}
*
**html5语义化兼容**4.21
document.createElement('leo');  
*input 新增表单控件和属性*
type :
email  :  邮箱
tel :  移动端会变为一个数字输入键盘
url :　网址　
search :  搜索 后面有个×
range : 数值选择器   ：
_
属性 step="2" 一次跳两步 min="0" max="10" value当前值
_
*新的输入形控件_2*
number : 只能包含数字的输入框
color : 颜色选择器
datetime : 显示完整日期
datetime-local : 显示完整日期，不含时区
time : 显示时间，不含时区
date : 显示日期
week : 显示周
month : 显示月
_新的表单特性和函数_
placeholder : 输入框提示信息
autocomplete : 是否保存用户输入值
           默认为no,关闭提示选择off
autofocus : 指定表单获取输入焦点      *自动聚焦*
list 和 datalist : 为输入框造一个选择列表
	- list值为datalist 标签的id
required : 此项必填 ， 不能为空
pattern : 正则验证 pattern = "\d{1,5}"
Formaction 在submit 里定义提交地址    又增加一个提交地址 可以的。。
maxlength  限制表单输入最大值


## imooc-why 重要的
项目地址：[响应式布局,慕课网教程](https://github.com/DemoorBug/comprehensive/tree/master/imooc-why)
> 这个估计是我所有东西的启蒙网站了，很值得留念

*为方便后续使用网址，在这里收集下*
```bash
.ico 在线转换 网址 http://www.bitbug.net
humans官方网址 http://www.humanstxt.org.cn
.editorconfig  http://editorconfig.org
markdown http://dillinger.io  很好的一个参考网站 http://www.jianshu.com/p/375bd2057d18
最新浏览器升级  http://browsehappy.com
HTML5 大纲生成页面   https://gsnedders.html5.org/outliner/
Normalize (NO 么赖似) https://necolas.github.io/normalize.css
css3自动增加前缀   http://autoprefixer.github.io
OwlCarousel2 轮播插件（O 凯弱塞奥2）官方http://owlcarousel2.github.io/OwlCarousel2/
```
*开始*
_robots.txt 机器人_ 搜索引擎查看网站,第一个访问的文件。告诉我们的爬虫服务器上什么文件，被查看，什么文件不能被查看
```bashUser-agent: *
Disallow: /admin/
此例子告诉搜索引擎都可以去访问,除了admin文件下的内容，每种不同的 搜索引擎都会有自己不一样的设置```
一些流氓的搜索引擎会无视次文件，毕竟这只是相关行业的约定，你不遵守，也没办法
_favicon.ico_  可以通过一些在线的网站进行转换，比如http://www.bitbug.net
_humans.txt 人类_ 就是一个单纯的文本文件，用来保存网站的建设者，和一些其他有用的信息，可以快速的了解网站背后的团队信息，开发人员信息，和他们的一些故事等等，也就是“*机器人*”是给搜索引擎、机器看的，*humans*是给我们人看的  官方网站http://www.humanstxt.org.cn  可以了解到，写法，想怎么写就怎么写，官网有参考范例
_.editorconfig_ 是用于统一代码解决方案的文件。很多项目都有用到  官网http://editorconfig.org  官网有相关规则，还有支持的IDE
```bash# editorconfig.org
# ; 注释 单独占用一行 一般使用#
# root = true 表示这是最顶层的配置文件，发现root=true的话 就不会再继续查找editorconfig文件了
root = true

# 一般看到*都知道是通配符  *代表所有文件，*.js 代表js文件
[*]
charset = utf-8
indent_size = 4
# indent_size = 4  4代表空格数  每次缩进的数 当然也可以设置为tab
# indent_size = tab 那么我们接下来会设置一个 tab的宽   tab-width = 多少  一般来说呢，我们都通过空格来进行缩进
indent_style = space
# indent_style = space  空格缩进  tab在各种编辑器宽度解释各有不同，为了保证代码排版看起来一致。建议都使用空格
insert_final_newline = true
# insert_final_newline = true   每一个文件 用空格行结尾
trim_trailing_whitespace = true
# trim_trailing_whitespace = true 去除换行行首的任意字符
[*.md]
trim_trailing_whitespace = false```
_.gitignore_
*安装呢 使用什么编辑器就在哪里安装，我安装到NPM里面了。。*
```bash*~
.DS_Store

.idea

node_modules
dist
这个是github 管理忽略的上传文件```
_LICENSE.txt_ 如果项目基于一些许可协议 可以建立这样一个文件，注意这个文件名全部使用了大写
_README.md_ 顾名思义呢，就是项目的简介、使用方式、相关链接
_CHANGELOG.md_ 看名字也都知道、项目每个版本的更新、说明的版本号、更新内容、修复了哪些问题等
*以上两个文件都是使用 .md作为后缀， md == markdown  是一种普通的文本编辑器，来编写的一个标记语言，通过一些简单的标记语法呢，就可以让普通的一些文本具有一定的格式，*
编辑markdown 在线 编辑网站 http://dillinger.io   很好的一个参考网站http://www.jianshu.com/p/375bd2057d18
*编写代码*
_lang="zh-CN"_  中文简体
_lang="zh"_ 支持更广泛的中文字符，简体，繁体，方言啊等等的(方言是什么鬼)
_lang="en"_ 这样的话 问题也不是很大，但是你安装了google翻译啊什么的  他会自动翻译 *还有些针对盲人的朗读软件 如果设置为en 可能就不会正常的工作 可能出现中文字符就跳过了，不读*
_< meta http-equiv="x-ua-compatible" content="ie=edge">_ x-ua-compatible == IE的兼容性  通过此代码表示在IE下的呈现方式 IE8开始多了个兼容模式 为了在IE8显示不正常，但在老的浏览器下显示正常的一些模式 IE9再往后也会支持这样一个模式 比如我们把  content="ie=EmulateIE7" 设置成这样代表模仿IE7。
x-ua-compatible如果此标签设置不正确的话。就会有几率出现用IE5.5 来渲染，

ie=edge 表示强制以最新的IE模式渲染页面
*注意 : IE11这个文档模式已经弃用了*
_< meta name="viewport" content="width=device-width,initial-scale=1">_ 设备宽度统一，初始缩放比1

_ < !--[if gt/lt/gte/lte IE 8]>< ![endif]-- > _  IE的条件注释
```bashgt == 选择这个版本以上的版本
lt == 比当前版本低
gte ==  可以理解为>=
lte == 可以理解为<=
*IE 8 这里要加空格 老师说后面有讲到*```http://browsehappy.com
_IE8不兼容HTML5 媒体查询 如何兼容呢  后面会有提到_
*编写基本内容，及HTML5标签语义化详解*
```bashnav 导航
article (啊提Q) 代表文档啊 页面或者应用程序中独立的完整的一些可以被外部引用的一些内容，怎样理解呢，一般可以是博客里面的一篇文章，一个帖子
section (塞克什) 实际使用中呢其实经常会和 article 这样一个元素搞混，区别：section使用范围更广泛，每一个区块都可以去使用，比如页面里面的广告，联系方式，文章里面的章节，只要觉得是个区块就可以使用，一般来说呢元素内容明确出现在文档大纲里section就比较适用，比如说装饰就不用了，但是你是需要出现在文档大纲中，需要让用户看到的，有意义的内容。
article 可以看做一个特殊的section，它是section的一个子集对section一个特殊的描述，他比section具有明确的语义，如果你是一个独立的完整的有内容的一个相关的区块，比如说包含一个图文内容的文章，这块确实是言之有物，就可以使用article
*怎么理解呢article相比section 他的语义更加的强烈它可以脱离整体，作为一个完整部分的存在，举个例子来说一个广告，你嵌在首页里面有意义，你把它拿出来放到别处没有什么太大的意义，但是一篇文章呢，独立出来和嵌到页面都可以，都有单独的意义，甚至于这篇文章呢你把它放在rss里面去订阅 也可以独立的存在 当然广告不会放在rss里面去作为一个订阅存在*
nav 元素呢也是一种特殊的 section 应用在导航里面呢你就一般不会吧导航写成section 而是把导航写成nav 其实article也是一样的 具有更强的语义
*如果不太确定就用div (D物)*
其他标签的一些语义规定
b 吸引人注意，不传达任何比如重要啊，强调啊
em 重点强调
i 和其他文字 文法和其他文法不同可以使用```
*编写代码*
注意：
    一般都使用class定义样式，id一般用于js快速的区别和获取元素class，一般都用中横线分割，id一般使用驼峰名称法
    *class='main-top' id='mainTop'*
_必不可少的图片使用< img> 引入，可有可无的装饰性图片可以使用标签的style引入_
这是一个 HTML5 大纲生成页面  https://gsnedders.html5.org/outliner/
*WebStorm* 自带了一个生成大纲插件  HTML5 Outline
*section 最高有标题 生成大纲后 吧没有标题的 section 换成 div*
*css resets VS Normalize.css*
resets  (锐塞似)
Normalize (NO 么赖似)   https://necolas.github.io/normalize.css
*px,em,rem*
_px_ 1个px相当于1个像素

_em_ 相对的长度对象，参考物 父元素的font-size 父元素没有的话就找到最接近的font-size为主，页面没有设置font-size的话 1em == 16px

_rem_ rem相对参照物为根元素html，相对参照固定不变所以好计算  IE9 里面只要不在font里面简写就没有问题，但是IE8不兼容
*清除浮动*
_flex_ 是一种可以替代float布局的方式  IE支持差
*CSS中有个概念 BFC 块级格式化上下文，简单理解就是 触发了BFC就会闭合浮动 预防高度塌陷 能触发BFC的属性：*
*解决inline-block 间距问题*
```bash
< ul>
    < li>< a href="#">登录< /a>
    < li>< a href="#">快速注册< /a>
    < li>< a href="#">关于< /a>
    < li>< a href="#">帮助< /a>
    < li>< a href="#">App下载< /a>
< /ul>
        _不加结尾  浏览器会自己加 亲测IE5.5 以上_
< ul>
    < li>< a href="#">登录< /a>
< /li>< li>< a href="#">快速注册< /a>
< /li>< li>< a href="#">关于< /a>
< /li>< li>< a href="#">帮助< /a>
< /li>< li>< a href="#">App下载< /a>
< /li>
< /ul>
        _这种是将 闭合标签添加到下一行 也可以解决_

< ul>
    < li>< a href="#">登录< /a>< /li>< li>< a href="#">快速注册< /a>< /li>< li>< a href="#">关于< /a>< /li>< li>< a href="#">帮助< /a>< /li>< li>< a href="#">App下载< /a>< /li>
< /ul>
        _写在一行，有些丑之外，都还不错_
父元素设置 font-size:0 ; 子元素设置font-size: * ; 某些情况下也有很多问题，例如老师这个例子就出现了下边距

header .top ul li + li {
  border-left: 1px solid #999;
  margin-left: -3px;
}
_通过-3px设置的，也有问题，每个浏览器间距不一样 导致出现差异_
*CSS4草案中有这样一个属性：*
white-space-collapsing 这个属性可以解决这个设置，但是CSS4啊，。。只能展望未来了
```
*CSS3 calc()*
calc是CSS3的一个计算方法。动态计算

_\00a0\00a0_ 代表不换行的空格字符
```bash.notice a:first-child:before {
  content: '最新公告: \00a0\00a0';
  color: #aaa;
}```
_文字不换行- 显示省略号_
```bash.notice a:first-child {
  text-overflow: ellipsis;
  overflow: hidden;
  white-space: nowrap;
}```
http://autoprefixer.github.io  自动增加前缀
*用户代理字符串*
_navigator.userAgent_ 可以通过此代码  显示当前用户代理字符串，当然安全性不好，用处：判断用户当前是安卓，还是IOS 从而给出相应的下载链接
*媒体查询常用节点*
```bash/* 800px或者以下 包含800 */
@media only screen and (max-width: 800px) {
    header .top {
        background-color: green;
    }

}
/* 481和800之间 */
@media only screen and (min-width: 481px) and (max-width: 800px) {
    header .top ul li a {
        color: red;
    }

}
/* 小于等于480像素 */
@media only screen and (max-width: 480px) {
    header .top ul li a {
        color: blue;
    }
}```
*媒体查询的单位选择*
*媒体查询 不会根据html font-size大小改变rem大小，也就是说他会按照浏览器的默认1rem == 16px*
_rem_  兼容性不好，而且是根据浏览器大小限制，这里没有优势
_em_ 兼容好，根据浏览器字体大小改变，完美
_px_ 这种用户改变浏览器默认字体大小，就会出现bug
*CSS3选择器*
_CSS选择器介绍_
```bash一般分为3类
①：
_基本选择器_
*   通配符 选择所有元素 或者某个元素下的所有元素
E   元素选择器
.class  类选择器
#id  ID选择器
E F  后代选择器 也被称作包含选择器
E > F   子元素选择器 只能选择某个元素的子元素 不包含孙子元素 或再往后的元素
E + F   相邻兄弟选择器 可以选择紧接在另一个元素后的元素 而且他们具有一个相同的父元素 也就是说呢E和F具有一个相同的父元素  而且呢 F在E的后面而且相邻
E ~ F   通用元素选择器，也是选择某个元素后的元素，和相邻兄弟选择器非常的像，举个例子
< ul>
    < li>< /li>
    < li>< /li>
    < a>< /a>
    < li>< /li>
< /ul>
ul li + li 选中了 后面的一个li
ul li ~ li 选中了 后面所以的li
②：
_属性选择器_
E[attr]               a[href] 只要 a 有 href 就都被选择
E[attr="value"]       指定属性名 和属性值  可以不加引号 那就得按规范来 不能以数字啊，两个短横线开头等等
E[attr^="value"]      以value 开头的  a[href ^= "http://"] 都被选中
E[attr$="value"]      value 结尾的   img[src$='.png']
E[attr*="value"]      只要包含就OK img[src*='.png']  a[href *= "http://"]  不管在什么位置出现都行
E[attr~="value"]      title= "hello world"  a[title ~= "world"] 以空格隔开的都可以模糊查询到
E[attr|="value"]      和 ^差不多 但是| 还包含以-开头的  lang="zh-cn"  a[lang|="zh"]
*其中 *=  = 最为常用*
③：
_伪类和伪元素的选择器_
:link 链接 :visited 被访问的链接 :hover 鼠标悬浮 :active 被集活 :focus 表单元素变为焦点
:enabled 启用禁用被选中的文本框 :disabled  checked 被选中的
:first-child   某个元素的第一个子元素
:last-child    某些元素的最后一个子元素
:nth-child()   选中一个或多个特定的子元素  ()内呢 可以写整数值，也可以写表达式：2n  n是从0开始 2,4,6 简写的方式even 偶数单词  奇数单词odd 2n+1 或 2n-1
:nth-last-child()
:nth-of-type()
*和  :nth-child() 区别对比 ：*
    < div class="nth">
        < div>1< /div>
        < p>2< /p>
        < p>3< /p>
        < p>4< /p>
        < p>5< /p>
        < p>6< /p>
    < /div>
.nth p:nth-child(2) {
    color: red;              2内容变红 从nth向下找到第二个元素 并且是P
}
.nth p:nth-of-child(2) {
    color: blue;             3变蓝 从nth下的子元素 P开始 计算，第二个P
}





:nth-last-of-type()
:first-of-type
:last-of-type
:only-child

.nth p:only-child  {          当只有一个P的时候 变红
    color: red;
}

:only-of-type              也是一样 不过是找类型
:empty                     空元素 什么也没有 哪怕一个空格
:not()
.nth :not(p)  {            所有不是P的元素
    color: red;
}

_伪元素选择器：比较老的伪元素选择器，css2的时候就已经有了_

:first-line  第一行
:first-letter 第一个文字
:before 前 :after 后


_第三个隐藏_
ul li:nth-child(3) {
    display:none
}
```
*小屏样式及技巧*
_手机屏幕再小也不会小于 320 PX_

*轮播图，我们不需要重复的造轮子*
所以呢 选择一款适合的插件
这里用到的：
_OwlCarousel2 （O 凯弱塞奥2）_ 升级版本
官方网址 http://owlcarousel2.github.io/OwlCarousel2/
GitHub https://github.com/OwlCarousel2/OwlCarousel2
*响应式图片*
< picture>
    < source srcset="images/ad002-l.png" media="(min-width:50em)">
    < source srcset="images/ad002-m.png" media="(min-width:30em)">
    _< source srcset="images/ad002-m.png" media="(orientation: landscape)" >_ 这个表示横屏
    < img src="images/ad002.png">  <!-- 视口宽度 viewport width缩写 -->
< /picture>

在线编辑SVG文件    http://editor.method.ac  https://icomoon.io

*polyfill (泡里废奥)*  可以吧裂缝填平，兼容性坑填

_picturefill (陪个扯儿废由)_  这就是picturefill库 http://scottjehl.github.io//picturefill

_SVG图片压缩_ http://iconizr.com

_PNG JPG..压缩_ https://tinypng.com

*NPM 技巧*
Node.js使用了一个事件驱动、非阻塞式I/O的模型(非阻塞的读取文件  I/O 读写)
Node.js 中文网  http://Nodejs.cn
Node.js 官网  http://Nodejs.org
_NPM 命令_         NPM 网站  https://npmjs.com
```bashnpm install jquery
package.json  (配K只)
_建立 package.json_
npm init
name: (src) 不能大写 包名
version: (1.0.0) 主版本号 副版本号 补丁版本号
description: 描述
entry point: () 入口文件
test command: 测试命令
git repository: git上的路径
keywords: 关键字
author: 作者
license: (ISC)  这个包基于什么样的一个协议
_packages.json 文件解读_
dependencies  这里面所写是   生产环境下的包

devDependencies 开发过程所依赖的包
_使用packages.json_
npm i  简写         npm i --production 下载产品方面依赖的包  npm i --dev 下载开发过程所依赖的包
npm install

_查看版本_
* -v
* --version

_管理员权限运行_
windows 直接右击 管理员权限就行
mac os 中sudo npm install gulp
_如何更新到packages.json_
npm install jquery --save-dev --save   前者是写到devDependencies  后者dependencies

npm uninstall jquery --save --save-dev 一个道理

npm update jquery 更新
npm update 更新所有
```

*http-server*
github https://github.com/indexzero/http-server

启动 http-server 根目录 参数

*虚拟机  选择*
github   https://github.com/xdissent/ievms

或者微软官方  https://developer.microsoft.com/en-us/microsoft-edge/tools/vms/

*移动端测试*
手机使用趋势 http://www.umindex.com/
安卓设备虚拟机 http://genymotion.net

*兼容调试 hack *
可以查看到各种浏览器之间的 hack写法 http://browserhacks.com

html5Shiv https://github.com/aFarkas/html5Shiv

Respond IE6到8不支持CSS3查询  https://github.com/scottjehl/Respond

modernizr.js  https://modernizr.com   主动性检测兼容问题  *防御性编程*

兼容问题千千万 使用任何特性之前 到 http://caniuse.com网站查询兼容性

*提高效率*

当有多个设备时 ， 改完一个样式每个页面都要刷新， 效率低，刚好有这么一个 ，省时的浏览器同步测试工具
http://www.browsersync.cn
NPM 安装到 全局就可以了
sudo npm install -g browser-sync
_命令_
```bashbrowser-sync start  启动服务  后面可以跟两个参数， --server "src" 服务器根目录设置到什么下 --files "src" 监听这个文件夹的所有改动 可以写一些通配符 "**/*.css"
browser-sync start  --server "src" --files "src"
```

*打包发布*
在线网站  https://javascript-minifier.com  在线压缩代码

主流的3个工具

1.Grunt
                  自动化构建工具
2.Gulp

3.Webpack         静态资源打包工具


_gulp (告 铺)_
官网 http://gulpjs.com  中文网 http://gulpjs.com.cn


用到的插件
```bash
gulp-rev              给每个文件计算 哈希码 然后把文件更改掉
gulp-rev-replace      文件名改变 index引用则改变
gulp-useref           注释 怎样的合并方式
gulp-filter           可以把JS文件筛选出来做一些压缩，然后再放回去
gulp-uglify           比较出名的 压缩 js代码插件
gulp-csso             就是压缩CSS的一个插件
--save-dev  肯定是开发过程中使用的

_var jsFilter = filter('**/*.js', {restore: true});_
_var cssFilter = filter('**/*.css', {restore: true});_
_var indexHtmlFilter = filter(['**/*','!**/index.html'], {restore: true});_
这个任务里面声明三个 filter (非由特)
然后 return 一个这样的东西
    return gulp.src('src/index.html')
        .pipe(useref())
        _分析带有标注的地方 例如 < !-- build:js scripts/combined.js -->_
        这样他就把 这些文件扔到流里面了，这样我们的流就包含这些文件了
        .pipe(jsFilter)
        _接下来该做我们的 filter 处理了，第一个filter就是把我们的JS文件筛选出来_
        .pipe(uglify())
        _做一个 uglify的操作，也就是做一个压缩的操作_
        .pipe(jsFilter.restore)
        _通过这样在扔回到流里面 jsFilter.restore。接着源代码的流接着往下流_
        .pipe(cssFilter)
        .pipe(csso())
        .pipe(cssFilter.restore)
        .pipe(indexHtmlFilter)
        _这里['**/*','!**/index.html']可以是一个数组，第一个意思是所有文件，第二个！是排除的意思，为什么排除我们的index呢，因为处理之后首页最好还是叫index。不然你加个版本号，主页就没了。。_
        .pipe(rev())
        _生成哈希版本号文件名_
        .pipe(indexHtmlFilter.restore)
        _之后呢再恢复_
        .pipe(revReplace())
        _恢复后呢。我们对index中的引用进行更新_
        .pipe(gulp.dest('dist'));
        _最后呢可以看到这样一个方法dest()这里呢就是结束了，把我们管道里的水(也就是文件流)给扔到这个目录下_
*这就是一个典型的gulp任务*
可以看到是一系列的 pipe (牌普) 处理
```

_gulp default 这个是默认任务执行 ，当然默认也可以不写_

_*! 这样的注释在 gulp压缩中 不会被压缩掉_

*gulp 实现眼花缭乱的功能*
_gulp-watch_  监听文件的改版，自动改变，
_gulp-postcss _ 可以把 (奥吐普锐菲克斯) 结合，自动给浏览器属性添加一些前缀，也可以对我们的CSS文件进行一些处理
_gulp-concat_ 可以把我们的多个文件合并成一个文件
_gulp-responsibe_ 这是一个可以实现响应式图片的文件 可以把我们的一个大图片，按照我们的规则 生成一系列的响应式图片，这些过程都是自动化的

*这些插件还有很多，我们可以在NPM里面输入gulp- 。 一般来说在一个项目的生命周期中，很少回去改变，属于一个一劳永逸的事情，所以很有必要花些时间写一个完美的gulpfile文件*
*趁手的IDE*

_IDE_ 简单的来说就是集成开发环境

*响应式框架*
_Bootstrap_    http://getbootstrap.com         http://v3.bootcss.com
_Foundation_   http://foundation.zurb.com         http://www.foundcss.com
结构语义化，移动设备优先，完全可以定制
_Semantic UI_  http://semantic-ui.com         http://semantic-ui-cn.com
命名非常的语义化，
_PureCss_      http://Purecss.io         http://purecss.org
非常轻量级

*WebStorm*


## svg 没有内容，忘记干什么的了
[svg](https://github.com/DemoorBug/comprehensive/tree/master/svg)

## CSS 既 html 标签收集
[CSS 既 html 标签收集](https://github.com/DemoorBug/comprehensive/tree/master/%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C)

W3C标准(1.1)  http://www.w3.org/TR/SVG11
浏览器支持情况 http://caniuse.com/#cats=svg
*ie margin-right 有双margin  bug  display: inline;解决 *
**Node.js收集**
http://www.nodebeginner.org/index-zh-cn.html#javascript-and-nodejs
_事件驱动的回调_

**电商网站收集**
_
	*font:italic small-caps bold 12px/1.5em arial,verdana; *
等效于： 
font-style:italic; 
font-variant:small-caps; 
font-weight:bold; 
font-size:12px; 
line-height:1.5em; 
font-family:arial,verdana; 
顺序：font-style | font-variant | font-weight | font-size | line-height | font-family 
（注：简写时，font-size和line-height只能通过斜杠/组成一个值，不能分开写。） 
不过使用这种简写需要注意几点：要使简写定义有效必须至少提供 font-size 和 font-family 这两个属性 ；同时font-weight, font-style 以及 font-varient 这几个属性如果不做设定的话将默认为normal。
_
_
	& copy; == ©
	gt; == >
	lt; == <
_
_input的背景需要清楚none _
_line-height: 35px\9;_  所有IE浏览器
*css hack*  好多地方都有提到
_IE 最小高度20 overflow: hidden解决_
_IE 百分比宽度不精确  可以用 *width:49.9% 类似方案解决_
_都给border-bottom  的话  IE会撑出  margin-bottom : -1px; position:relative;提升层级_
_元素撑到下一个位置的时候  可以考虑设置高度来解决_
**走进svg**
_基本API_
创建图像
documen.createElementNS(ns,tagName);
添加图像
element.appendChild(childElement)
设置/获取属性
element.setAttribute(name,value)
element.getAttribute(name)
*rx圆角半径 cx横坐标 *
< rect >、  x  y   width height rx ry   这个属性画矩形
< circle >、 cx cy r      r
< ellipse >、 cx cy rx ry    椭圆
< line >、    x1 y1 x2 y2 直线
< polyline >、 x1,y1 x2,y2 ... 折现      points="x1 y1 x2 y2 x3 y3"
< polygon >    多边形  就是讲最后第一个点和 起始点连接
基本属性
fill、stroke、stroke-width、transform
**2016.4.19**
transform: scale(0.3);   这个是放大效果
transform-origin: center;
transition-delay: 0;  延迟 很有用
**2016.4.18**
autoprefixer  一个库
lessc --source-map ***.less ***.css  绑定文件 更好找到less编译后的css
wr --exec "lessc --cource-map ***.less ***.css" ***.less  当less 更改后执行该命令  --exec有待研究
https://www.npmjs.com/package/wr 参考网站
**svg 气泡学习**2016.4.15
outline css样式（轮廓）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。
article   html5标签 
*< article> 标签规定独立的自包含内容。
一篇文章应有其自身的意义，应该有可能独立于站点的其余部分对其进行分发。
< article> 元素的潜在来源：
论坛帖子
报纸文章
博客条目
用户评论*
section  < section > 标签定义文档中的节（section、区段）。比如章节、页眉、页脚或文档中的其他部分。

## CSS
[css](https://github.com/DemoorBug/comprehensive/tree/master/%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C)
**置顶不知道的属性**
letter-spacing
word-spacing
vertical-align
**vertical-align家族基本认识**5.31
inherit		继承
1。线类
	baseline,top,middle,bottom
2.文本类
	text-top,text-bottom
3。上标下标类
	sub,super
4.数值百分比类     //IE6/7 不支持小数行高
	20px,2em,20%

## Mysql笔记
[mysql笔记](https://github.com/DemoorBug/comprehensive/tree/master/%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C)

ob_clean 清除输出图片前的空格
[client] [mysqld]下添加default-character-set=utf8
**51**.
//     print_r(mysql_fetch_row($result));  //速度最快
//     print_r(mysql_fetch_assoc($result));
//     echo mysql_field_name($result, 3);
//     mysql_field_name — 取得结果中指定字段的字段名
//     echo mysql_num_fields($result);
//     mysql_num_fields — 取得结果集中字段的数目
/*
     * mysql_get_client_info — 取得 MySQL 客户端信息
     * mysql_get_host_info — 取得 MySQL 主机信息
     * mysql_get_proto_info — 取得 MySQL 协议信息
     * mysql_get_server_info — 取得 MySQL 服务器信息
     */
     mysql_num_fields($result);//字段数目
     mysql_num_rows($result);//行数目
     *很多优化语句*

**50**.
require '50Demo.php';   //大概就是将其他PHP建立连接吧(可以调用函数之类的)
/*     *//新增数据*
    $query = 'INSERT INTO user(name,email,point,regdate) VALUE ("王大大","2358@163.com","150",NOW())';
    mysql_query($query); */
    
/*     *//删除修改*
    $query = 'UPDATE user SET name="王" WHERE id="4" ';
    mysql_query($query); */
    
/*     *//显示数据*
    $query = 'SELECT id,name,email,point,regdate FROM user';
    $result = mysql_query($query);
    while ($row = mysql_fetch_array($result)) {
        echo $row[0],$row[1],$row[2],$row[3].'<br/>';
    }
     */

**47，48，49**.
* 这章包含了很多知识点*
mysql_connect — 打开一个到 MySQL 服务器的连接
mysql_query('SET NAMES UTF8');  //设置字符集
mysql_select_db — 选择 MySQL 数据库
mysql_query — 发送一条 MySQL 查询 //就是执行mysql语句
mysql_fetch_array — 从结果集中取得一行作为关联数组，或数字数组，或二者兼有 
mysql_free_result — 释放结果内存
mysql_close — 关闭 MySQL 连接
**46**.
   /*
     *              WHERE   表达式的常用运算符
     *     MYSQL运算符                                     含义
     *      =                                                       等于
     *      <                                                       小于
     *      >                                                       大于
     *      <=                                                   小于或等于
     *      >=                                                   大于或等于
     *      !=                                                     不等于
     *      IS NOT NULL                                     具有一个值
     *      IS NULL                                         没有值
     *      BETWEEN                                     在范围内
     *      NOT BETWEEN                             不再范围内
     *      IN()                                                   指定的范围
     *      OR                                               两个条件语句之一为真
     *      AND                                              两个条件语句都为真
     *      NOT                                              条件语句不为真
     *      LIKE                                                    筛选
     */

    /*
     * SELECT * FROM user WHERE email LIKE '%163.COM';*  %代表任意 筛选出163.com结尾的
     *   SELECT * FROM user WHERE email LIKE 3;  //一个参数代表显示前3条
     *   *SELECT * FROM user WHERE email LIKE 2,3;*  //代表从3开始显示3个
     * SELECT * FROM user OREDR BY regdte DESC;  regdte按照这个进行倒叙
     * *SELECT AVG(point) AS '平均值' FROM user;*  算出平均值
     * 
     */
/* *SELECT AVG(point) AS '平均值' FROM user;* 算出平均值
 *                                     MYSQL分组函数
 *                函数                    用法                  描述
 *                AVG()             AVG(column)        返回列的平均值
 *                COUNT()       COUNT(column)       统计行数  (NULL统计不到)
 *                MAX()                                              求列的最大值
 *                MIN()                                               求列的最小值
 *                SUM()                                               求列中的和
 */
    /**检查这个表的信息*
    *   SHOW TABLE STATUS\G
    * *优化一张表*
    * OPTIMIZE TABLE grade;
    */
        *为什么表格式为iatin1_swedish_ci*
**45**.
 /*
     * 创建一个数据库，表grade内容ID,姓名NAME,邮件email,评分
     * point,注册日期regdate
     * *UNSIGNED 表示无符号，TINYINT(2) 无符号整数0-99,NOT NULL
     * 表示不能为空,AUTO_INCREMENT表示从1开始每增加一个字段，累加一位*
     * 
     */
    /*
     * *CREATE TABLE USER*(  设置主键 自动递增
     * ID TINYINT(2) UNSIGNED NOT NULL AUTO_INCREMENT,
     * NAME VARCHAR(3) NOT NULL,
     * EMAIL VARCHAR(40),
     * POINT TINYINT(3) UNSIGNED NOT NULL
     * PRIMARY KEY(ID) 设置主键 让ID唯一不得重复
     */
    *遗留问题 如何删除字段 添加指定字段*
**44**.
    /*
     * Mysql 常用函数
     *                                  文本函数
     *         函数                    用法                       描述                                                            大致写法
     *       CONCAT()       CONCAT(X,Y,...)         创建形如xy的新字符串                                 CONCAT(‘lee’,'ee')   leeee 
     *       LENGTH()       LENGTH(column)        返回列中储存的值长度
     *       LEFT()             LEFT(column,x)         从列的值中返回最左边的X个字符
     *       RIGHT()            RIGHT(column,x)        从列的值中返回最右边的X个字符
     *       TRIM()             TRIM(column)            从存储的值删除开头和结尾的空格
     *       UPPER()          UPPER(column)        吧存储的字符串全部大写
     *       LOWER()         LOWER(column)        吧存储的字符串全部小写      
     *       SUBSTRING() SUBSTRING(column,start,length)  从column中返回开始start的length个字符(索引从0开始)
     *       MD5()              MD5(column)              吧存储的字符串用MD5加密
     *       SHA()              SHA(column)              吧存储的字符串用SHA加密
     *                                   数字函数
     *                                  *有很多数字函数  这里只列举几个  可以到这章看*
     *                                 四舍五入  最大整数      最小整数  绝对值
     *                                 *日期和时间函数*
     *                                 HOUR() 返回存储日期的小时值  SELECT HOUR(NOW());
     *                                 
     *               *格式化日期和时间DATE_FORMAT() 和 TIME_FORMT()*
     *       名词                         用法                      示例
     *       %e                     一月中某一天              1~31
     *       %d                     一月中的某天，两位    01~31
     *       %D                     带后缀的天                   1st~31st
     *       %W                     周日名称                  Sunday~Saturday
     *                  *还有很多 就不一一列举了 格式化的比上面更细腻*
     *                      *SELECT DATE_FORMAT(NOW(),'%e')*
     */
**43**.对应章节43Demo2.PHP
//   Mysql 数据库操作
   /*
    * 显示当前存在的数据库
    * >*SHOW DATABASES;*
    * 2选择你所需要的数据库
    * >*USE guet*;
    * 3查看当前所选择的数据库
    * >*SELECT DATABASE()*;
    * 4查看一张表的所有内容
    * >*SELECT * FROM guest*   //可以先通过*SHOW TABLES*来查看有多少张表
    * >*SHOW TABLES;*
    * 5根据数据库设置中文编码
    * >*SET  NAMES gbk*       //set names utf8
    * 6创建一个数据库
    * 首先打开 *SHOW DATABASES*打开所有数据库看
    * *CREATE DATABASE book*;
    * 
    * 7在数据库里创建一张表
    * *CREATE TABLE uses(
    * username VARCHAR(20) ,//NOT NULL,这样的话 就不能为空 没有数据就出错
    * sex CHAR(1) ,  //性别
    * birth DATETIME);*  //注册日期
    * 
    *8 显示数据表结构
    **DESCRIBE uses;* 简写形式 DESC USES;  *那不早说*
    *  Field       Type              NULL              KEY              Default    Extra
    *   user        varchar(20)   YES                                     NULL
    *  字段名      类型           可以为空      主键未设置   默认值没有   额外的没有  no-replat
    *9 显示表的结构
    *>  *INSERT INTO uses (username,sex,birth) VALUES ('Lee','x','NOW()');*
    **NOW()* 是Mysql 自带的时间
    *仅显示 字段名呢  *没看懂*
    **SELECT username FROM uses;*
    *10 筛选指定的数据
    **SELECT * FROM uses WHERE sex='0';*
    *11 删除数据
    * *DELETE FROM user WHERE sex='0';*
    * 12修改指定数据
    * *UPDATE uses SET sex='男' WHERE USERNAME='LEE';*
    * 13按指定的数据库排序
    * *SELECT * FROM uses ORDER BY birth DESC;* //正序
    * *ASC*// 正序
    * 14删除指定的表
    * *DROP TABLE uses;*
    * 15删除指定的数据库
    * *DROP DATABASE book;*
    */
    /*
     * SELECT 应该是查看         *挑选*
     * SHOW 叫显示    *显示* 
     * USE  选择你想要的数据库  *使用* 
     * DATABASE()函数   DATABASES命令  *数据库*
     * *代表所有字段 SELECT * FROM guest
     * Empty set (0.00sec) 这是一张空表
     * 
     * 
     */
**43**.
    /*
     *                      日期型
     *      列类型                 ‘零’值
     *      DATETIME        '0000-00-00 00:00:00'
     *      DATE                '0000-00-00'
     *      TIMESTAMP      000000000000000
     *      TIME                   '00:00:00'
     *      YEAR                0000
     *      
     *                      字符串型
     *      值           CHAR(4)           存储需求          VARCHAR(4)      存储要求
     *      ''                  '     '                4个字节              ''                          1个字节
     *      'ab'            'ab   '                4个字节             'ab'                       3个字节
     *      'abcd'        'abcd'               4个字节            'abcd'                   5个字节
     *      'abcdefgh'  'abcd'              4个字节             'abcd'                   5个字节 
     *     *如果采用CHAR类型：* 那么就存放4个字节  空格也算字符
     *     CHAR 定长类型
     *     *VARCHAR 类型 * 会将后面的空格删掉；
     *     VARCHAR 可变长度类型 自己本身长度+1
     *     
     *                      *备注型*：自己本身+1
     *  类型                      描述
     *  TINYTEXT            字符串，最大长度255个字符
     *  TEXT                    字符串，最大长度65535个字符
     *  MEDIUMTEXT      字符串，最大长度16777215个字符
     *  LONGTEXT          字符串，最大长度4294967295个字符
     *      *text 比较常用 用于备注，大文章，帖子，新闻*

     *      *CHAR 一般用于密码  性别 加密之后永远是32 || 40*

     *      *VARCHAR 用于标题*
     *      
     *                  *整数型*
     *  类型                  字节    最小值                     最大值
     *                                   (带符号的/无符号的)   (带符号的/无符号的)  
     *  TINYINT             1           -128                        127
     *                                              0                        255
     *  SMALLINT         2           -32768                    32767
     *                                              0                        65535
     *  MEDIUMINT       3          -8388608                 8388607
     *                                              0                       16777215
     *    INI                    4          -2147483648           2147483647
     *                                              0                       4294967295
     *  BIGINT              8  -9223372036854775808   9223372036854775807
     *                                              0                       18446744073709551615
     *                         *整数型*      
     *   类型               字节         最小值                     最大值
     *   FLOAT              4       +-1.175494351E-38      +-3.402823466E+38
     *   DOUBLE           8     +-2.2250738585072014E-308   +-1.7976931348623157E+308
     *   DECIMAL         可变         它的取值范围可变
     */
**42**.
    /*
     * Mysql 常用命令
     * 1显示当前数据库的版本号和日期
     * SELECT VERSION();
     * CURRENT_DATE();
     * 2通过AS关键字设置字段名
     * SELECT VERSION() AS version;  // 可设置中文，通过单引号
     * 3通过SELECT执行返回计算结果
     * SELECT (20+5)*4 AS '结果';
     * 4 通过多行实现数据库的使用者和日期
     * SELECT USER(),NOW();   //一行显示
     * SELECT USER(); 使用者
     * SELECT NOW(); 
     * 5 通过一行显示数据库使用者和日期
     * SELECT USER() , SELECT NOW();
     * 6 命令取消
     * > \c
     * 7 MySql 窗口退出
     * > exit;
     * ctrl+C 
     * quit
     * 
     * MySql 常用数据类型
     *      整数型：TINYINT, SMALLINT , INT, BIGINT
     *      浮点型：FLOAT , DOUBLE, DECIMAL(M,D)
     *      日期型：CHAR, VARCHAR
     *      备注型：TINYTEXT, TEXT , LONGTEXT
     *      
     * 
     */
     原文：http://www.2cto.com/database/201108/101151.html

MySQL会出现中文乱码的原因不外乎下列几点：
1.server本身设定问题，例如还停留在latin1
2.table的语系设定问题(包含character与collation)
3.客户端程式(例如php)的连线语系设定问题
强烈建议使用utf8!!!!
utf8可以兼容世界上所有字符!!!!
一、避免创建数据库及表出现中文乱码和查看编码方法
1、创建数据库的时候：

[sql] view plain copy
CREATE DATABASE `test`  
DEFAULT CHARACTER SET 'utf8'  
COLLATE 'utf8_general_ci';  
2、建表的时候 
[sql] view plain copy
CREATE TABLE `database_user` (  
`ID` varchar(40) NOT NULL default '',  
`UserID` varchar(40) NOT NULL default '',  
) ENGINE=InnoDB DEFAULT CHARSET=utf8;  

这3个设置好了，基本就不会出问题了,即建库和建表时都使用相同的编码格式。
但是如果你已经建了库和表可以通过以下方式进行查询。
1.查看默认的编码格式:

[sql] view plain copy
mysql> show variables like "%char%";  
+--------------------------+---------------+  
| Variable_name | Value |  
+--------------------------+---------------+  
| character_set_client | gbk |  
| character_set_connection | gbk |  
| character_set_database | utf8 |  
| character_set_filesystem | binary |  
| character_set_results | gbk |  
| character_set_server | utf8 |  
| character_set_system | utf8 |  
+--------------------------+-------------+  

注：以前2个来确定,可以使用set names utf8,set names gbk设置默认的编码格式;
执行SET NAMES utf8的效果等同于同时设定如下：

[sql] view plain copy
SET character_set_client='utf8';  
SET character_set_connection='utf8';  
SET character_set_results='utf8';  


2.查看test数据库的编码格式:

[sql] view plain copy
mysql> show create database test;  
+------------+------------------------------------------------------------------------------------------------+  
| Database | Create Database |  
+------------+------------------------------------------------------------------------------------------------+  
| test | CREATE DATABASE `test` /*!40100 DEFAULT CHARACTER SET gbk */ |  
+------------+------------------------------------------------------------------------------------------------+  

3.查看yjdb数据表的编码格式:
[sql] view plain copy
mysql> show create table yjdb;  
| yjdb | CREATE TABLE `yjdb` (  
`sn` int(5) NOT NULL AUTO_INCREMENT,  
`type` varchar(10) NOT NULL,  
`brc` varchar(6) NOT NULL,  
`teller` int(6) NOT NULL,  
`telname` varchar(10) NOT NULL,  
`date` int(10) NOT NULL,  
`count` int(6) NOT NULL,  
`back` int(10) NOT NULL,  
PRIMARY KEY (`sn`),  
UNIQUE KEY `sn` (`sn`),  
UNIQUE KEY `sn_2` (`sn`)  
) ENGINE=MyISAM AUTO_INCREMENT=1826 DEFAULT CHARSET=gbk ROW_FORMAT=DYNAMIC |  


二、避免导入数据有中文乱码的问题
1:将数据编码格式保存为utf-8
设置默认编码为utf8：
set names utf8;
设置数据库db_name默认为utf8:

[sql] view plain copy
ALTER DATABASE `db_name` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;  
设置表tb_name默认编码为utf8:
[sql] view plain copy
ALTER TABLE `tb_name` DEFAULT CHARACTER SET utf8 COLLATE utf8_general_ci;  
导入：
[sql] view plain copy
LOAD DATA LOCAL INFILE 'C:\\utf8.txt' INTO TABLE yjdb;  
2:将数据编码格式保存为ansi(即GBK或GB2312)
设置默认编码为gbk：
set names gbk;
设置数据库db_name默认编码为gbk:
[sql] view plain copy
ALTER DATABASE `db_name` DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;  
设置表tb_name默认编码为gbk:
[sql] view plain copy
ALTER TABLE `tb_name` DEFAULT CHARACTER SET gbk COLLATE gbk_chinese_ci;  
导入：
[sql] view plain copy
LOAD DATA LOCAL INFILE 'C:\\gbk.txt' INTO TABLE yjdb;  
注：1.UTF8不要导入gbk，gbk不要导入UTF8;
2.dos下不支持UTF8的显示;
三、解决网页中乱码的问题
 
将网站编码设为 utf-8,这样可以兼容世界上所有字符。
　　如果网站已经运作了好久,已有很多旧数据,不能再更改简体中文的设定,那么建议将页面的编码设为 GBK, GBK与GB2312的区别就在于:GBK能比GB2312显示更多的字符,要显示简体码的繁体字,就只能用GBK。
1.编辑/etc/my.cnf　,在[mysql]段加入default_character_set=utf8;
2.在编写Connection URL时，加上?useUnicode=true&characterEncoding=utf-8参;
3.在网页代码中加上一个"set names utf8"或者"set names gbk"的指令，告诉MySQL连线内容都要使用
utf8或者gbk;


## Nodejs
[Nodejs](https://github.com/DemoorBug/comprehensive/tree/master/%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C)

call  作用域指向 两个参数
apply
*根_本_听不*懂*,_从_基础*来*_吧_ 加油*  
**重写爬虫，更优雅的异步编程**
*https*
	http	https    
	tcp	  ssl/tls
	ip        tcp
	           ip
*https*node中专门处理加密传输

**Promise学习使用**
*对象有3种状态*
未完成(pending)
已完成(fulfilled)
失败(rejected)
*A与A+不同点*
A+规范通过术语thenable来区别promise对象
A+定义onFulfilled/onRejected必须是作为函数来调用，而且调用过程必须是异步的
A+严格定义了then方法链式调用时onFulfilled/onRejected的调用顺序
*promise then方法*
promiseObj.then(onFulfilled,onRejected)   _第一个是操作时调用的回调函数，第二个是失败时调用回调函数_
onFulfilled = function(value){
	return promiseObj2
}
onRejected = function (err){
	
}
**Promise**promise/promise.html
安装npm install bluebird库	
_< script	scr="../node_modules/bluebird/js/browser/bluebird.js"></ script >_
**request方法**
get就是对request的一个封装  _也就是说get能干的事 request都能做_
*http.request(options[,callback])*
第一个参数是对象的话就可以配置 来定制我们发出的请求的格式
_host_ 将要请求的服务器域名，或是ip地址
_hostname_ 就是host的别名
_port_ 远端服务器端口 末日80
_localAddress_ 绑定local链接的本地接口
_socketPath_ 
_method_ 指定http请求方法的字符串  默认get
_path_ 请求的路径 默认就是根路径 如果是有查询的字符串就要在后面追究一些参数
_headers_ 包含请求头的一个对象
_auth_ 用于计算头的基本认证  u'ze pa si wo de
_agent_ 控制agent行为也就是一个代理
_keepAlive_ 保持资源池周围的套接字在未来能被用于其他请求 末日的值是false
_keepAliveMsecs_
**小爬虫**	/school/comment.js
*爬虫*
var http = require('http')
var url = 'http://www.imooc.com/learn/348'
http.get(url,function(res){
	var html = ''
	res.on('data',function(data){
		html += data
	})
	res.on('end',function(){
		console.log(html)
	})
}).on('error',function(){
	console.log('获取失败')
})
**HTTP源码解读**	
nodejs  高并发          
iojs 密集操作
作用域	  局部作用域 全局作用域  
上下文    this  一个函数被作用于对象调用时 this 指向调用这个方法的对象
var pet = {
	words: '...',
	speck : function (){
		console.log(this.words)
	}
}
pet.speck()
*this通常指向当前函数拥有者   拥有者叫做执行上下文*
pet.call(this,words);
pet.apply(this,arguments);
_this 代表js一个关键字 函数运行时自动生成的一个内部对象_
```html
nodejs  顶层的global"阁楼薄"对象 == window
```
**HTTP概念进阶**	
*什么是回调*

*什么是同步/异步*
var c=0;
function printIt() {
	console.log(c)
}
function plus(callback) {
	setTimeout(function(){
		c+=1;
		callback();

	},1000)
}
plus(printIt)
*什么是I/O*    磁盘  数据的进和出
*什么是单线程/多线程*
*什么是阻塞/非阻塞*  
*什么是事件*
*什么是事件驱动*
*什么是基于事件驱动的回调*
*什么是事件循环*
_可以通过回调的方式来达到异步编程，非堵塞的效果_
_nodejs  非堵塞  单线程  和事件驱动  更好的理解下一节内容_
**状态码**	
			             							200   OK成功
1xx   	请求接受，继续处理	             			400   语法错误 服务器无法理解
2xx		成功	             						401	  没有授权
3xx		重定向，要想完成就要更进一步操作(跳转或..)	403	  拒绝提供服务  可能还是没有权限
4xx		客户端错误，语法错误 ，无法实现	            404	  没找到
5xx		服务器端错误	           				  	500   服务器端发生了不可预期错误
			             							503   服务器端当前还不能处理这个请求                 
**http知识填坑-分解**

**http知识填坑**
*http头和正文信息  *
```
http头发送的是一些附加信息：内容类型
服务器发送响应的日期，HTTP状态码
正文就是用户提交的表单数据。
```
chrome//net-internals/#dns
首先域名解析

1  chrome 先搜索自身的DNS缓存
2  搜索操作系统自身的DNS缓存(浏览器没有找到缓存或缓存已经失效)   chrome//net-internals/#dns 查看缓存是否过期
3  读取本地host文件
4  浏览器发起一个DNS的一个系统调用
5  浏览器获得域名对应的IP地址后，发起HTTP"三次握手"
6  TCP/IP 连接起来后，浏览器就可以向服务器发送http请求了使用了比如说，用HTTP的GET方法请求一个根域里的一个域名，协议可以采HTTP1.0的一个协议
7  服务器端接受到了这个请求，根据路径参数，经过后端的一些处理之后，把处理后的一个结果数据返回给浏览器，如果是慕课网的页面就会把完整的HTML页面代码返回给浏览器
8  浏览器拿到了慕课网的HTML页面代码，在解析和渲染这个页面的时候，里面的js、css、图片静态资源，他们同样也是一个HTTP请求需要经过上面的主要的七个步骤
9  浏览器根据拿到的资源对页面进行渲染，最终把一个完整的页面呈现给用户
*运营商的DNS服务器呢首先*
1.查找自身的缓存找到对应的条目
2.没有找到会代替我们的服务器发起一个迭代DNS解析请求
	运营商服务器吧结果返回操作系统内核	同时缓存起来
	操作系统内核把结果返回浏览器
	最终浏览器拿到www.imooc.com对应的IP地址
**Query String参数处理小利器**
*querystring.stringify* 序列化
querystring.stringify({name:'scott',course:['1','2']},',',':')  第四个参数 默认限制1000 设置为0 就不限制
*querystring.parse* 反序列化

*querystring.escape('<哈哈>')*  转译
*querystring.unescape('%3C%E5%93%88%E5%93%88%3E')*
**URL解析好帮手**
node
*url.parse*
url.parse('http://www.imooc.com/video/6710')  如果第二个参数设置true  则 query将变为一个对象  第三个参数设置为true
{
protocol: 'http',         什么协议 http  ftp
slashes:true,			  是否//
auth:null,
host:'www.imooc.com',     ip地址或者域名
port:null, 				  端口 默认80
hostname:'www.imooc.com',  主机名
hash:null,                锚点
search:null,        查询字符串参数
query:null,			发送数据=号间隔开的  键值称之为参数传
pathname:'/video/6710', 	资源路径名
path:'/video/6710',			路径
href:'http://www.imooc.com/video/6710'  没被解析的超链接
}
*url.format*
url.format({
protocol: 
slashes:
auth:
host:
port:
hostname:
hash:
search:
query:
pathname:
path:
href:
})
_会将这些解析为 合法的 url地址_
*url.resolve('http://imooc.com/','/course/list')*
http://imooc.com/course/list
**命令行体验**
 目前版本 0.10.34  飙升 0.10.35  io.js也同时发布了1.0.1的版本 标志nodejs分化为两个阵营
**起一个web服务器**
const http = require('http');

const hostname = '127.0.0.1';
const port = 3000;

const server = http.createServer((req, res) => {   请求体和响应体    _req：那个url地址过来的，请求类型 res:告诉服务器给这个请求响应一些内容 要不然一直就是挂起的状态_       
  res.statusCode = 200;		//返回的状态码  代表成功								
  res.setHeader('Content-Type', 'text/plain');		 //返回的文本内容类型是 纯文本
  res.end('Hello Nodejs\n');        
   _这里是匿名的回调函数_
});

server.listen(port, hostname, () => {
  console.log(`Server running at http://${hostname}:${port}/`);
});
_修改内容没变化 因为没有重启服务器_
*有请求有返回  五脏俱全啊*


## Nodejs入门
[]

```js
这个链接呢 是node学习的根基 没有它  就没有我 哈哈
```
<a>http://www.nodebeginner.org/index-zh-cn.html</a>
**处理POST请求**
*这里的特定的事件有data事件（表示新的小数据块到达了）以及end事件（表示所有的数据都已经接收完毕）。*
```js
我们需要告诉Node.js当这些事件触发的时候，回调哪些函数。怎么告诉呢？ 我们通过在request对象上注册监听器（listener） 来实现。这里的request对象是每次接收到HTTP请求时候，都会把该对象传递给onRequest回调函数。
request.addListener("data", function(chunk) {
  // called when a new chunk of data was received
});

request.addListener("end", function() {
  // called when all chunks of data have been received
});
```
**以非阻塞操作进行请求响应**

**堵塞与非堵塞**
*上述代码中，我们引入了一个新的Node.js模块，child_process。之所以用它，是为了实现一个既简单又实用的非阻塞操作：exec()。*
```js
exec()做了什么呢？它从Node.js来执行一个shell命令。在上述例子中，我们用它来获取当前目录下所有的文件（“ls -lah”）,然后，当/startURL请求的时候将文件信息输出到浏览器中。

上述代码是非常直观的： 创建了一个新的变量content（初始值为“empty”），执行“ls -lah”命令，将结果赋值给content，最后将content返回。
```
**路由给真正的请求处理程序**
```js
路由，顾名思义，是指我们要针对不同的URL有不同的处理方式。例如处理/start的“业务逻辑”就应该和处理/upload的不同。
```
*我们暂时把作为路由目标的函数称为请求处理程序。*
```js
应用程序需要新的部件，因此加入新的模块 -- 已经无需为此感到新奇了。我们来创建一个叫做requestHandlers的模块，并对于每一个请求处理程序，添加一个占位用函数，随后将这些函数作为模块的方法导出：
function start() {
  console.log("Request handler 'start' was called.");
}

function upload() {
  console.log("Request handler 'upload' was called.");
}

exports.start = start;
exports.upload = upload;
这样我们就可以把请求处理程序和路由模块连接起来，让路由“有路可寻”。
```
*不过结果有点令人失望，JavaScript没提供关联数组 -- 也可以说它提供了？事实上，在JavaScript中，真正能提供此类功能的是它的对象。*
```js
在C++或C#中，当我们谈到对象，指的是类或者结构体的实例。对象根据他们实例化的模板（就是所谓的类），会拥有不同的属性和方法。但在JavaScript里对象不是这个概念。在JavaScript中，对象就是一个键/值对的集合 -- 你可以把JavaScript的对象想象成一个键为字符串类型的字典。
但如果JavaScript的对象仅仅是键/值对的集合，它又怎么会拥有方法呢？好吧，这里的值可以是字符串、数字或者……函数！
```

**行为驱动执行**
*函数式编程*
**路由模块**
	我们需要的所有数据都会包含在request对象中，该对象作为onRequest()回调函数的第一个参数传递。但是为了解析这些数据，我们需要额外的Node.JS模块，它们分别是url和querystring模块。
```js

                               url.parse(string).query
                                           |
           url.parse(string).pathname      |
                       |                   |
                       |                   |
                     ------ -------------------
http://localhost:8888/start?foo=bar&hello=world
                                ---       -----
                                 |          |
                                 |          |
              querystring(string)["foo"]    |
                                            |
                         querystring(string)["hello"]
当然我们也可以用querystring模块来解析POST请求体中的参数，稍后会有演示。

```
**服务端的模块放在哪里**
*exports可以导出函数*

**服务器是如何请求的**
```js
当我们的回调启动，我们的onRequest()函数被触发的时候，有两个参数被传入：request和response，他们都是对象，你可以使用他们的方法来处理http请求的细节(比如向发出请求的浏览器发回一些东西)
```
```js
_今天先到这里。。明天贼尴尬。。居然没去成。。平常上班你怎么那么准时，到了这种时候却起不来，啊啊啊。。我都不信啊，我也是有毒。。居然没定闹钟。。额 。老天爷。你为什么要这样。。好吧 。。今天还看了看东西。给自己点赞+1_ 2016/7/4
```
_开始_
```js
所以我们的代码就是：收到请求，使用response.writeHead()函数发送一个状态200和HTTP头的内容类型(content-type),使用response.write()函数在HTTP相应主体中发送文本"hello World",最后我们调用response.end()完成响应
```
*目前来说，我们对请求的细节并不在意，所以我们没有使用request对象*

**基于事件驱动的回调**
为什么我们要用这种方式呢(上一章节)
_这个问题不好回答，不过这是nodejs原生的工作方式。他是事件驱动的，这也是他为什么这么快的原因_
*作者的理解，我自己也理解写下*
```js
当我们使用http.createServer方法的同时，我们当然不只是想要一个侦听某个端口的服务器，我们还要它在服务器收到一个http请求的时候做点什么。
```
问题来了，这是异步的：请求任何时候都可能到达，但是我们服务器却跑在一个单进程中。
```js
php的时候可以一点都不用担心，任何时候当有请求进入的时候，网页服务器(通过apache)就为这一请求新建一个进程，并且开始从头到尾执行相应的PHP脚步
```
那么在nodejs程序中，当一个新的请求到达8888端口的时候，我们怎么控制流程呢
```js
我们不知道这件事情什么时候发生，但是我们现在有一个处理请求的地方：他就是我们传递过去的那个函数。至于他是被预先定义的函数还是匿名函数，就无关紧要了。这个就是传说中的*回调*。我们给某个方法传递一个函数，这个方法在有相应事件发生时调用这个函数来进行回调
```
让我们再来琢磨琢磨这个新概念。我们怎么证明，在创建完服务器之后，即使没有HTTP请求进来、我们的回调函数也没有被调用的情况下，我们的代码还继续有效呢？我们试试这个：
```js
var http = require("http");

function onRequest(request, response) {
  console.log("Request received.");
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}

http.createServer(onRequest).listen(8888);

console.log("Server has started.");
```
_执行这串代码后  第一次回出现 Server has started.
当访问这个页面时 就会在命令行输出 Request received. 这就是事件回调_
*这就是事件驱动的异步服务器端JavaScript和它的回调啦！*
```js
*（请注意，当我们在服务器访问网页时，我们的服务器可能会输出两次“Request received.”。那是因为大部分服务器都会在你访问 http://localhost:8888 /时尝试读取 http://localhost:8888/favicon.ico ) *
```
**创建server.js文件**
```js
var http = require("http");

http.createServer(function(request, response) {
  response.writeHead(200, {"Content-Type": "text/plain"});
  response.write("Hello World");
  response.end();
}).listen(8888);

```
_http://localhost:8888/，_
*分析HTTP服务器*
第一行是nodejs 自带的一个http模块  将其赋值给 http变量
_接下来调用http模块提供的函数: createServer 。这个函数返回一个对象，这个对象有一个叫做listen的方法，这个方法有一个数值参数，指定http服务器监听的端口号_
我们本来可以用
```js
var http = require("http");

var server = http.createServer();
server.listen(8888);
```
这样的代码启动服务器
_他的第一个参数是传递一个函数啊 怪不得_
```js
function say(word) {
  console.log(word);
}

function execute(someFunction, value) {
  someFunction(value);
}

execute(say, "Hello");
来个例子吧   挺叼的哈
```
我们不一定要绕这个先定义，在传递的圈子。
```js
function execute(someFunction, value) {
  someFunction(value);
}

execute(function(word){ console.log(word) }, "Hello");
作者意思是我们还是得遵循渐进么，先接受一点，在js中，一个函数可以作为另一个函数接收一个参数，我们可以先定义一个函数，然后传递，也可以在传递参数的地方直接定义函数。
```

## PHP
[PHP](https://github.com/DemoorBug/comprehensive/tree/master/%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C)

ctrl+shift+j  注释函数
查看快捷键	ctrl+shift+l
**41**.对应文件41Demo2.php
*这章主要讲解了些 图片进行微缩*
*getimagesize* — 取得图像大小
创建一个新图  $im = imagecreatetruecolor($_width,$_height);
下面的工作是载入原图  $_im = imagecreatefromjpeg(__RIL__.'12.jpe');
 imagecopyresampled — 重采样拷贝部分图像并调整大小

 //将新图输出
    imagejpeg($im,null,10);
    imagedestroy($im);    //关闭第一个句柄
    imagedestroy($_im);		//关闭第二个句柄
**41**.
*加载系统已有字体*
define('URL', dirname(__FILE__).'\\');
header('Content-Type:image/png'); - 设置开头
*imagecreatefrompng *— 由文件或 URL 创建一个新图象。
采用系统提供的字体
*iconv* — 字符串按要求的字符编码来转换
*imagecolorallocate* — 为一幅图像分配颜色
*imagettftext* — 用 TrueType 字体向图像写入文本
*这个是自己写的随机验证码  感觉不错*
*imagepng* - 输出
*imagedestroy* - 清除
**40**.对应章节40Demo2.php
*这章导入一个已有的图片 添加水印*
define('URL', dirname(__FILE__).'\\');
*imagecreatefrompng — 由文件或 URL 创建一个新图象。*
**40**.
*简单的一个验证码*
// mt_rand — 生成更好的随机数
//     echo mt_rand(0,15);
//  为什么要循环0-15之间的数呢？
//  因为要实现最简单的字母和数字搭配
//  十六进制 0-9  a-f;
//  dechex — 十进制转换为十六进制
//  十六进制 0-9都一样的  但是10-15的时候会转换为A-F
//     echo dechex(mt_rand(0,15));
    for ($i=0;$i<4;$i++) {
        @$nmsg .= dechex(mt_rand(0,15));  //会报错 也解决不了 头疼
    }

**39**.
/*
     * 创建图像的一般流程
     * 1.设定标头，告诉浏览器你要生成的MIME类型
     * 2.创建一个图像区域，以后的操作都将基于此图像区域
     * 3.在空白图像区域绘制填充背景
     * 4.在背景上绘制图像轮廓输入文本
     * 5.输出最终图像。
     * 6.清除所有资源
     * 7.其他页面调用图像
*/ 
//  *1设置标头指定MIME输出类型：*
    	header('Content-Type: image/png');
//  *2新建一个基于调色板的图像*
	$im = imagecreatetruecolor(200, 200);
//  *为一幅图像分配颜色*
	$color = imagecolorallocate($im, 153,204 ,255 );
//  *3区域填充*
	imagefill($im, 0, 0, $color);
//  *4绘制线条*
	$white = imagecolorallocate($im, 0,0,0 );
	imageline — 画一条线段
	imageline($im, 0, 0, 200, 200, $white);
	imagestring — 水平地画一行字符串
	imagestring($im,5,80,80,'Mr.jj',$white);
//  *5以 PNG 格式将图像输出到浏览器或文件*
	imagepng($im); //要放一个句柄
//  *第六步 清除所有资源*
	imagedestroy($im);

**38**.
创建一个常量
dirname — 返回路径中的目录部分
is_dir — 判断给定文件名是否是一个目录
 mkdir — 新建目录
*define('URL', dirname(__FILE__).'\images');*
$UR = str_replace( '\\', '/',URL);
in_array — 检查数组中是否存在某个值
is_array — 检测变量是否是数组
*这个章节进行了很多优化和改进*
if (!is_dir(URL)) { *这个创建一个常量后，进行各种取值取得目录，如果没有该目录 则自己创建*
       mkdir(URL,0777);
}
**37**.
*这两章都写到一起了，36主要讲了一些根目录PHP.ini的设置*
**36**.
*这一章用处很多*
is_uploaded_file()  判断文件是否通过HTTP POST上传的
move_uploaded_file — 将上传的文件移动到新位置
location.href='xianshi.php?url=".'images/'.$_FILES['username']['name']."'</ script>"
*大概就是可以将一个图片用户去上传到本网站，成为自己网站的一部分，差不多就是可以达成我们说的  B 2 B 用户对用户*
**35**.
做一个简单的登陆
*cookie*
*Session会话*
**34**.
time — 返回当前的 Unix 时间戳
setcookie('name','wq');
/*  *cookie限制*
 * 1、必须在HTML文件的内容输出之前设置
 * 2、不同的浏览器对cookie的处理不一致，且有时会出现错误的结果
 * 3、限制是在客户端的。一个浏览器能创建的cookie数量最多为30
 * 个，并且每个不能超过4KB，每个WEB站点能设置的cookie总量
 * 不超过20个
 */
 *$_COOKIE*
**33**.
接收所有数据
正则判断邮箱
PHP可以输出JS代码
*$_GET 可以接收超链接？后的变量值*
**32**.
header('Content-Type:text/html;charset=gbk');//设置页面编码
*header('Location:Demo2.php'); //绝对相对链接都可以*
/*
 *  表单元素                 描述
 *  text input              文本框
 *  passoword input     密码框
 *  hidden                   隐藏框
 *  select                      下拉列表框
 *  checkbox                复选框
 *  radio                       单选按钮
 *  textarea                    区域框
 *  file                            上传
 *  submit                     提交按钮
 *  reset                         重置按钮
 *  email 				邮箱
 */
 *$_POST  $_GET*
 isset — 检测变量是否设置
**31**.
explode — 使用一个字符串分割另一个字符串
microtime — 返回当前 Unix 时间戳和微秒数  返回两个值
localtime — 取得本地时间  
date_default_timezone_set('Asia/Shanghai');  //有效 //字符串意思 ： 亚洲/上海
putenv — 设置环境变量的值  putenv('TZ=Asia/Shanghai')  这种设置已经无效了  不知道为什么 反正用上面那个就行了
getlastmod — 获取页面最后修改的时间 :
date('Y-m-d H:i:s',getlastmod()+(60*60*8)) 这种写法后面60*60*8 == 已秒数增加8小时
strtotime('2015-12-05 18:13:06');  //人性化获取时间戳
mktime(12,18,2,1,7,2016);   //这种就很反人类吧  //时-分-秒-月-日-年
date('Y-m-d H:i:s',mktime(12,18,2,1,7,2016))
time()+(60*60*8); //时间差写法
round — 对浮点数进行四舍五入
**30**.
checkdate — 验证一个格里高里日期  //没有详细的用法 
gettimeofday — 取得当前时间  //返回数组
getdate — 取得日期／时间信息
time — 返回当前的 Unix 时间戳  //直接获取时间戳
**29**.
preg_grep — 返回匹配模式的数组条目 //函数搜索数组中的元素。
preg_match — 执行一个正则表达式匹配 //第一个当然是正则表达式 第二个要进行匹配的字符串
preg_match_all — 执行一个全局正则表达式匹配  //要传3个参数 第三个参数利用二维数组储存
	//定界正则
preg_quote — 转义正则表达式字符
preg_replace — 执行一个正则表达式的搜索和替换
	//取消贪婪
1.第一种是利用JS中的?方式
2.第二种呢PHP中自己有  用  "U"  直接去除贪婪

preg_split — 通过一个正则表达式分隔字符串
print_r(preg_split('/[.w]/','qweq.qwe@qwe.q@we@qw.qwe'));
**28**.
preg_match — 执行一个正则表达式匹配
x表示忽略空白 /php/x;
**27**. 
	同28章  
**26**.
所有处理中文字符串的都是已mb开头的 
重点讲5个，其他可以硬套
 /*
	 * mb_strlen();     mb_strlen — 获取字符串的长度
	 * mb_strstr();      mb_strstr — 查找字符串在另一个字符串里的首次出现
	 * mb_strpos();    mb_strpos — 查找字符串在另一个字符串中首次出现的位置
	 * mb_substr();    mb_substr — 获取字符串的部分
	 * mb_substr_count();  mb_substr_count — 统计字符串出现的次数
	 *                                  //对应函数 substr_count();   手册中没有
	 * substr_count — 计算字串出现的次数                                
	 */
	$inname = '名字';
	mb_strpos($inname,'字',0,'gbk');  可以用这个格式硬套其他的 手册看了就会了
**25**.
	/*
	 * strcmp();  strcmp — 二进制安全字符串比较
	 * strcasecmp();  strcasecmp — 二进制安全比较字符串（不区分大小写）
	 * strnatcmp(); strnatcmp — 使用自然排序算法比较字符串
	 */
strcmp('吧', '啊');  // $st1 > str2 返回大于0 $st1 < $st2 返回小于0
strcasecmp('a', 'A'); //不区分大小写
strcmp('2', '10');  //这种是字符比较 所以比较的是第一个值
strnatcasecmp('2', '10');  //这个是自然排序法
strspn — 计算字符串中全部字符都存在于指定字符集合中的第一段子串的长度。
echo strspn('sin', 'suin po',0,3);
strlen($string); //字符串长度
substr_count — 计算字串出现的次数
strchr() 是 strstr的别名;
strstr('qweqdd', 'd',true);  //打印出该字符首次出现位置
strpos — 查找字符串首次出现的位置 //最先出现的位置
strrpos — 计算指定字符串在目标字符串中最后一次出现的位置 //最后出现的位置
// str_replace — 子字符串替换
// str_ireplace — str_replace() 的忽略大小写版本
// substr_replace — 替换字符串的子串
//     echo str_replace('.','伍德','this is .');
//     echo substr_replace('this is .','$#',0,5); // 第0个开始取5个 替换为$#
**24**.
/*explode() explode — 使用一个字符串分割另一个字符串
     * implode() implode — 将一个一维数组的值转化为字符串
     * join()  join — 别名 implode()
     */
strtok — 标记分割字符串
substr — 返回字符串的子串
str_split — 将字符串转换为数组
strrev — 反转字符串         *中文不行*
**23**.
trim — 去除字符串首尾处的空白字符（或者其他字符）
ltrim — 删除字符串开头的空白字符（或其他字符）
rtrim — 删除字符串末端的空白字符（或者其他字符）
   /*
    * chop()  chop — rtrim() 的别名
    * ltrim()  ltrim — 删除字符串开头的空白字符（或其他字符）
    * rtrim()  rtrim — 删除字符串末端的空白字符（或者其他字符）
    * trim()  trim — 去除字符串首尾处的空白字符（或者其他字符）
    */
nl2br — 在字符串所有新行之前插入 HTML 换行标记 //通常在回帖的时候使用
htmlentities 转换所有字符
htmlspecialchars($ea);  //转换html代码
strip_tags($ea);  //此方法直接将 < strong>删除
addslashes($ea);   //  给特殊符号加上转义符
stripcslashes($ea);  //将反斜杠去掉
//  格式化字符串大小写
    /*
     * strtoupper()  函数将字符串转换为大写
     * strtolower()   函数将字符串转换为小写
     * ucfirst()         函数将第一个字母转换为大写
     * ucwords()      函数将每个单词第一个字母转换为大写;
     * 
     * str_pad — 使用另一个字符串填充字符串为指定长度
     */
str_pad — 使用另一个字符串填充字符串为指定长度  *应该不错*
**22**.
require 'p.php';  //如果不存在就停止执行并报错
include 'p.php';    //如果不存在就报错
require_once 'p';  //如果调用一次 则不会继续调用
//推荐使用require;
魔法常量  ...

require(dirname(__FILE__).'\21Demo.php');  //此写法可以读取读取大目录
**21**.
按引用传递加上&即可
global 定义全局变量
$GLOBALS['a'] = 50;   //超级全局变量，就是一个数组啊
include '';  //==链接了一个Php文件吧
print_r($_POST);  不知道为什么用到这个
**20**.
	这一章貌似没什么重点 所以当时是跳过了
**19**.
file_exists — 检查文件或目录是否存在
filesize — 取得文件大小
unlink — 删除文件
rewind — 倒回文件指针的位置
fseek — 在文件指针中定位
ftell — 返回文件指针读/写的位置 //查看当前指针位置
fopen — 打开文件或者 URL
fgetc — 从文件指针中读取字符
fclose — 关闭一个已打开的文件指针
文件锁定  --解决问题:假如同时两个客户购买，只能获得一条数据
flock — 轻便的咨询文件锁定
opendir — 打开目录句柄
readdir — 从目录句柄中读取条目
closedir($dir); //关闭
scandir — 列出指定路径中的文件和目录
rmdir — 删除目录
rename — 重命名一个文件或目录
md5 — 计算字符串的 MD5 散列值  //加密
md5_file — 计算指定文件的 MD5 散列值
函数传入值的话 直接覆盖 没传值用赋值的
**18**.
fwrite — 写入文件（可安全用于二进制文件）
fgetc — 从文件指针中读取字符
fgets — 从文件指针中读取一行
fpassthru — 输出文件指针处的所有剩余数据
//读出文件
/*     
	fgetc($handle); — 从文件指针中读取字符
	fgets($handle); — 从文件指针中读取一行
	fgetss($handle); — 从文件指针中读取一行并过滤掉 HTML 标记
	fread($handle, $length); — 读取文件（可安全用于二进制文件）
	fpassthru($handle); — 输出文件指针处的所有剩余数据
	
	file($filename); — 把整个文件读入一个数组中
	readfile($filename); — 输出一个文件
	file_get_contents($filename); — 将整个文件读入一个字符串 //函数源码自带fclose 关闭 
	*/
//利用while循环完成  feof 测试文件指针是否到了文件结束的位置
**17**.
fopen — 打开文件或者 URL
fopen 返回的是资源类型(resource),我们一般称它为句柄，或者叫资源句柄
fwrite — 写入文件（可安全用于二进制文件）
file_put_contents — 将一个字符串写入文件 //不需要任何其他灵活的事情的时候使用
**16**.
basename — 返回路径中的文件名部分
dirname — 返回路径中的目录部分
pathinfo — 返回文件路径的信息
realpath — 返回规范化的绝对路径名
相绝路径
filesize — 取得文件大小
round — 对浮点数进行四舍五入
round(filesize($name)/1024,2);         //这样却得是字节，字节/1024得KB
disk_free_space — 返回目录中的可用空间
echo round(disk_free_space('D:')/1024,2)/1024/1024;
disk_total_space — 返回一个目录的磁盘总大小
echo round(disk_total_space('G:')/1024/1024/1024,2);
fileatime — 取得文件的上次访问时间
date_default_timezone_set — 设定用于一个脚本中所有日期时间函数的默认时区
fileatime — 取得文件的上次访问时间
filectime — 取得文件的 inode 修改时间
filemtime — 取得文件修改时间
date('Y:m:d '.'H:i:s',fileatime($Basic5));  //应该是可以直接打印出访问时间
**15**.
array_unshift — 在数组开头插入一个或多个单元 
array_push — 将一个或多个单元压入数组的末尾（入栈） 
array_shift($userNames); //删除开头
array_pop($userNames); //删除末尾
array_rand — 从数组中随机取出一个或多个单元 
array_rand($userNames,2);  //返回数组中随机或多个键(key) 

gettype — 获取变量的类型

current($array);   //获取指针当前的元素
next — 将数组中的内部指针向前移动一位
reset — 将数组的内部指针指向第一个单元
prev — 将数组的内部指针倒回一位
pos — current() 的别名
end — 将数组的内部指针指向最后一个单元
array_count_values($array)  统计数组中所有值出现的次数
extract — 从数组中将变量导入到当前的符号表  //将数组中字符串键(key)设置为变量
初始化 $z=$ci='';
**14**.
sort — 对数组排序
asort — 对数组进行排序并保持索引关系
foreach 语法结构提供了遍历数组的简单方式。foreach 仅能够应用于数组和对象，如果尝试应用于其他数据类型的变量，或者未初始化的变量将发出错误信息。有两种语法： 
each — 返回数组中当前的键／值对并将数组指针向前移动一步 
while ($a = each ($fn)) {     //按顺序打印
        echo '< br />'.$a['key'].'=>'.$a['value'];
}
count — 计算数组中的单元数目或对象中的属性个数
krsort — 对数组按照键名逆向排序
sort(); asort(); ksort();  a的是排序后不改变key关系
k是按key排序
rsort(); arsort(); krsort();  全部+上r 代表降序
shuffle — 将数组打乱
array_reverse — 返回一个单元顺序相反的数组 
rsort — 对数组逆向排序

//array打头的一般创建一个新数组进行返回 当然是一般喽
**13**.
list — 把数组中的值赋给一些变量 
each — 返回数组中当前的键／值对并将数组指针向前移动一步 
13章还有个小练习，虽然很简单，但是也是一番努力得来的
**12**.
reset — 将数组的内部指针指向第一个单元
array_unique — 移除数组中重复的值
array_unique($userNames);   //只能临时删除不能改变原数组
array_flip — 交换数组中的键和值
**11**.
each — 返回数组中当前的键／值对并将数组指针向前移动一步 
**10**.
range — 建立一个包含指定范围单元的数组 
	/*
	 *  count()和sizeof()统计数组下标的个数
	 *  array_count_values($array);统计数组内下标的个数
	 */
is_array — 检测变量是否是数组
**9**.
可以在变量或值前面增加(int)、(integer)、(float)、(double)或(real)实现
intval — 获取变量的整数值
floatval — 获取变量的浮点值
is_numeric — 检测变量是否为数字或数字字符串 
is_int($var);   is_float($var);  用于检测具体数值类型
/* 
    $a = '10.5';
//     echo intval($a);
    echo floatval($a);          //临时转换
    settype($a, "float");    //转换
    echo gettype($a);           //查看类型
 */
rand();     //这个是老的函数生成 随机数;
mt_rand(0,10);  //比rand快4倍  牛B啊
getrandmax();    //最大随机数
mt_getrandmax();    //最大随机数
	//格式化数据
number_format — 以千位分隔符方式格式化一个数字
number_format($a,2,'#','c');  //这里后面必须传两个参数才可全部更改
/*
 *  数学函数
 *  abs();  绝对值
 *  floor(); 舍去法取整 
 *  ceil(); 进一法取整
 *  round(); 四舍五入
 *  min(); 求最小值或数组中最小值
 *  max(); 求最大值数组中最大值
 */
echo pow(2,4);   //2的4次方
**8**.
switch ($w) { 
      case 1:
            echo '1';
            break;
	default :
             echo '没有';
             break;
}
//第一种：break  退出循环；第二种 exit 退出程序  ；第三种 continue 退出当前循环
break;   //退出循环;
exit;         //直接后面的程序都不执行了
continue;  //退出此次循环，继续执行
**7**.
/*
 * 转义序列                                         描述
 *  \n                                          换行符
 *  \r                                              回车
 *  \t                                              水平制表图   相当于tab
 *  \\                                              反斜杠
 *  \$                                              美元符
 *  \"                                              双引号
 */
错误抑制符@
**Demo**.  对应章节Baisa2/Demo2.php
gettype($var);    却得它的类型
settype($var, $type);  设置它的类型
isset();  和 unset()用来判断一个变量是否存在
empty(); 用来判断一个变量是否为空
换句话说，""、0、"0"、NULL、FALSE、array()、
    var $var ;(这个是空的变量，只不过写成VAR而已)
    以及没有任何属性的对象都将被认为是空的
unset — 释放给定的变量
isset — 检测变量是否设置
empty — 检查一个变量是否为空
	//类型判断函数
is_integer — is_int() 的别名
is_int — 检测变量是否是整数
//可以通过调用一个函数来实现转换变量数据类型的目的
     * intval()、floatval()、strval();
**Demo**.  对应章节Baisa2/demo.php
print — 输出字符串
printf — 输出格式化字符串
sprintf — Return a formatted string
sprintf("保留到内存中");  //返回字符串
**6**.
    //超级全局变量
/*
 * $GLOBALS        所有全局变量数组
 * $_SERVER         服务器环境变量数组
 * $_GET                通过GET方法传递给该脚本的变量数组
 * $_POST               通过POST方法传递给该脚本的变量数组
 * $_COOKIE         cookie数组变量
 * $_FLLES            与文件上载相关的变量数组
 * $_ENV                环境变量数组
 * $_REQUEST        所有用户输入的变量数组
 * $_SESSION        会话变量数组
 */
define — 定义一个常量
除了自定义常量外，PHP还预定了许多常量。了解这些常量的简单方法就是运行phpinfo();命令




应用场景	快捷键	功能
查看快捷键	ctrl+shift+l	显示所有快捷键列表
查看和修改快捷键	　	打开Window->Preferences->General->keys
修改字体字号	　	打开Window->Preferences->General->Appearance->Colors and Fonts，找到Basic->Text Font
行操作	ctrl+alt+↓	复制当前行到下一行
ctrl+d	删除当前行
ctrl+↓	当前行下移一行
ctrl+↑	当前行上移一行
end	跳转到当前行末尾
ctrl+Backspace 	删除光标前一个单词，如前面是符号，就删除一个符号,前面是一个单词就删除一个单词
alter+→	在编辑过的位置前进
alter+←	在编辑过的位置后退
ctrl+home	光标移到文件头
ctrl+end	光标移到文件尾
shift+home	选中从光标处到行首的文字
shift+end	选中从光标处到行末的文字
ctrl+/（数字键）	收起/展开代码段
shift+enter	在当前行的下一行插入空行
ctrl+shift+enter	在当前行的前一行插入空行
ctrl+Q	定位到最后编辑的地方
ctrl+shift+X 	当前选中的文本全部变为大写
ctrl+shift+Y	当前选中的文本全部变为小写
注释操作	ctrl+/	添加/取消行注释
ctrl+shift+/	块注释
ctrl+shift+\	取消块注释
函数操作	Alt+/	代码提示助手
ctrl	函数跳转，按住ctrl键，鼠标点击函数名
Ctrl+Shift+J 	给自定义函数或者方法添加注释
面向对象操作	ctrl+O	快速大纲, 列出当前文件中的所有变量、函数和方法，对阅读类文件时很有用
ctrl+shift+M	搜索方法名
ctrl+T	快速显示当前类的继承结构
查找操作	ctrl+f	打开本文件的搜索/替换 ，只搜索当前文件
ctrl+k	查找下一个
ctrl+shift+k	查找上一个
ctrl+h	全文检索，打开搜索替换窗口 ，可搜索整个工作空间
ctrl+l	转到当前文件的某一行
CTRL+E	搜索编辑器中已打开的文件名
文件操作	ctrl+w	关闭当前文件
ctrl+m	当前编辑窗口最大化/还原
ctrl+N	新建文件
ctrl+p	打印当前文件
ALT+ENTER	查看当前文档的属性
ctrl+F12	打开任务(任务：个人定义某一个特定的工作集,如你要完成一个注册模块，有三个文件config.phpregister.class.php register.php|你可以将这些文件保存成一个任务register ,只要打开register就能同时打开这三个文件| )
ctrl+F9	激活任务
ctrl+shift+F9	取消任务
F11	调试当前文件
ctrl+F11	运行当前文件
alt+→	下一个编辑的页面
alt+←	前一个编辑的页面
ctrl+shift+E	显示当前打开的所有的文件
代码格式化	Tab	增加代码缩进
Shift+Tab	减少代码缩进
CTRL+SHIFT+F	当前文件代码格式化
调试快捷键	F5	单步调试进入函数内部（单步进入）
F6	单步调试不进入函数内部（跳过）
F7	由函数内部返回到调用处（跳出）
F8	一直执行到下一个断点
F9	添加/删除断点 所有代码部分
F10	逐过程。单步执行调试文件到下一行
F11	逐语句。单步执行到下一被执行的行
F12	概要文件URL。打开profile URL对话框
Shift+F8	添加监视点。打开添加监视点对话框
Shift+F11	跳出。单步执行到返回后执行的第一行
Shift+F10	执行代码到光标所在行。
Ctrl+F5	无中断的执行脚本
Shift+F5	停止调试器
Ctrl+Alt+B	在浏览器中显示

## Jquery总结
[Jquery总结](https://github.com/DemoorBug/comprehensive)

toggleClass(“class1″)           *如果原来没有class1就添加class1，如果原来有class1就移除class1*
attr(“className”,”class1″)     
addClass(“class1″) .removeClass(“class2″)     *addClass 添加class  removeClass(“class2″) 移除*
_      移除所有
$("div").removeClass(function() {
  	return $(this).attr('class');
});	
_
*   添加递归元素   index可以获取递增数
自 jQuery 1.4开始,  .addClass() 方法允许我们通过传递一个用来设置样式类名的函数。
$("ul li:last").addClass(function(index) {
  return "item-" + index;
});
*
_
	toggle 绑定两个或多个处理程序绑定到匹配的元素，用来执行在交替的点击。
_
**前面所学 制作鼠标拖拽**4.21  11：31
_
$.fn.extend({
drag : function () {
	var aDiv = 0,
		cDIv = 0,
		This = this;
	this.mousedown(function(ev){
		aDiv = ev.pageX - $(this).offset().left;
		cDIv = ev.pageY - $(this).offset().top;
		$(document).mousemove(function(ev){
			console.log(aDiv)
			This.css('left',ev.pageX - aDiv);
			This.css('top',ev.pageY - cDIv);
			console.log('s')
		});
		$(document).mouseup(function(){
			$(this).off()
		})
		return false;  //不知道有什么用
	})
}
})
$('#drag').drag()
_
**$   插件**4.21
$
	-$.extend        扩展工具方法下的插件形式  $.xxx()
	$.extend({
		leftTime : function(ind){
			return ind.replace(/^\s+/,'');                    //leftTime为去掉左空格方法
		}
	})
$.fn
  	-$.fn.extend       扩展到jQ对象下的插件形式   $().xxx()
**$下的方法**4.21
ajax() : json形式的配置参数
-url success
-error contentType
-data type
-dataType cache timeout
抽象出来的方法
*   参数是一个json形式
	$.ajax({
		url : ‘地址’，
		data : 'name=hell&age=20',
		type : 'POST',
		success : function (data) {
			data 请求成功后返回的一个内容
			成功后的回调函数
		},
		error : function () {
			失败的回调
		},
		contentType : 请求头信息,
		dataType : 返回数据类型,
		cache : 是否缓存,
		timeout : 是否有延迟

	})
*
-get()
-post()
-getJSON()
**$下的方法**4.20
_$下的方法_       不仅可以给jq对象用  还可以用于原生js  _工具方法_
type()                 $.type('les');       检测参数类型   更加强大  可以区分[]  data  正则  null
trim() 				$.trim(' |            les      |'); 去空格   
inArray()            和indexof 差不多  $.inArray('b',arr)    arr= ['a','b','c']   有的话返回相应位置  没有从头开始数
proxy()             改变this指向        $.proxy(函数,指向) () 直接可以去执行   $.proxy(函数,指向,参数) (参数) 直接可以去执行
noConflict()       避免冲突    jquery  == $  别人的库说不定也是使用的这个 为了避免冲突 var miaov = $.noConflict()
parseJSON()     将字符串 解析为json类型     直接就可以解析ajax数据   $.parseJSON( "{'name':'names'}"  ).name
makeArray()    把类数组转换为真正的数组    类数组就是 var aDiv = document.getElementsByTagName('div'); 
这就叫做类数组             $.makeArray(aDiv).push();这样就不会报错了

**动画**4.20
animate()               //第一个参数是个json  {}  第二个参数时间 第三个参数 运动形式 默认：swing慢快慢   linear匀速    第四个参数回调   可以写链式调用  == 回调函数
stop()                     停止运动  默认只会知足当前运动   stop(true) 阻止后续运动  第二个参数  直接到达目标点 
*finish() 立即停止到你所有你指定的目标点*
delay()                   延迟  参数秒算起
delegate()             事件委托    
_
$('ul').delegate('li','click',function(){
		$(this).css('background','red');
})
_
undelegate()          $('ul').undelegate()  阻止事件委托
trigger()                   //主动触发    $('ul').delegate('li','click',function(){alert('w')})    *$('ul').trigger('click');*
ev.data                 
ev.target
ev.type
_
on('click',{name:'hello'},function(ev){
	ev.data  就是这个json {name:'hello'} 这个整体
	ev.data.name          ==  'hello'
	ev.target   当前事件源  我操作的是谁
	ev.type  当前操作的事件类型
})
_
**DOM操作**4.20
parents()                //当前操作节点的所有祖先节点，参数筛选功能
closest()                //获取最近指定的祖先节点(包含当前元素自身)，必须要写筛选的参数
siblings()               //获得所有的兄弟节点   有参数，筛选功能
nextAll()                 //下面所有的兄弟节点
prevAll()                 //上面所有的兄弟节点
parentsUntil()         //  _Until 截止的意思_        所有祖先节点 指定位置
nextUntil()             //截止到你指定的位置 就不会继续了  *当然是填写参数*
prevUntil()         
clone()                  可以接受一个参数，作用复制之前的操作行为    参数为_布尔值_
 //$(div).clone().appendTo( $('span') );  //这个属性用于复制节点
wrap()                  包装       $('span').wrap('< div >');    给span元素包装div
wrapAll()               整体包装   将集合中的元素 外围整体嵌套一个div  *元素不在一起 将元素剪贴过来 更改DOM节点*
wrapInner()         内部包装     div添加到了span 节点里
unwrap()               删除包装  删除父级
add()                     将元素组合到一起  进行后续操作   $(div).add('span').css('background','red')  两个元素都更改        
slice()                     选取的意思  有一组集合   $(div).slice(2,5) 第二个到第四个
serialize()            _可以将input    Name  Value  穿连到一起  有助于数据提交 get post_
serializeArray()      *还可以直接转换为数组形式*
**项目中应用**
siblings()   获得匹配元素集合中每个元素的兄弟元素,可以提供一个可选的选择器。。
animate()   根据一组 CSS 属性，执行自定义动画。
size()     length == size()
.parent() 取得匹配元素集合中，每个元素的父元素，可以提供一个可选的选择器。
toggleClass()  在匹配的元素集合中的每个元素上添加或删除一个或多个样式类,取决于这个样式类是否存在或值切换属性。即：如果存在（不存在）就删除（添加）一个类。
.stop()  停止匹配元素当前正在运行的动画。
.fadeIn() 通过淡入的方式显示匹配元素。
.children()   获得匹配元素集合中每个元素的子元素，选择器选择性筛选。
.contents()  获得匹配元素集合中每个元素的子元素，包括文字和注释节点。
**基础篇**:
get()  :  下标和length属性      *就是把jq转换为原生的js*    _$('div').get(0).innerHTML _
outerWidth() : 针对隐藏元素和参数true    _可以获取隐藏元素的 width height_
text() :   合体的特例        *和html()区别在于 只能获取文本 不能获取标签*
remove() : detach()           _var oDiv = remove() 删除所有操作  $('body').append(oDiv)进行恢复 也是无法完成的   detach() 删除了 但是它保留了所有操作_
$() : $(document).ready()

**17**:
$()方法
hover()            *hover 移入的效果 模拟css中的hover   两个参数 移入移出*  
show()             _隐藏    传参  时间参数_    改变高和display
hide() 		    *显示*
fadeIn() 		_淡入  同意有时间 默认400_
fadeOut() 		*淡出*       $('div').fadeOut(500);          改变透明度
fadeTo()            _半透明_  fadeTo(900,.5,function(){alert('d') }
slideDown() 	*向下展开*
slideUp() 	     _向上卷曲_
**13**:
_ jq 中的 禁止右击
	$(document).ready(function(){
		    $(document).bind("contextmenu",function(e){
		        return false;      //即阻止冒泡，又阻止默认事件
		    });
		});
_
$()常用方法
ev 	      	*//鼠标事件 event对象*
pageX 		 _ev.pageX     获取当前鼠标位置_
which          *键盘键值*
preventDefault             _阻止默认事件_
stopPropagetion 		*阻止冒泡*
one()                           _事件执行一次_
offset() 					*$('div').offset().top;  获得元素距离屏幕的左距离* 算margin
position() 					_从定位后算起  父元素设置定位  就从父元素算起_ 
offsetParent() 			*获取有定位的父级*
val() 						_获取val值  加参数后== 修改_
size() 					*类似于length  获取长度*
each()  					_就是jq中的循环_
*
	$('li').each(function (i,value) {
			alert(i +'--'+ $(value).html());
		})
*
_   拖拽
$(document).ready(function () {
		var disx = 0;
		var disy = 0;
	$('div').mousedown(function (ev) {
		disx = ev.pageX - $(this).offset().left ;
		disy = ev.pageY - $(this).offset().top ;

		$(document).mousemove(function (ev) {
			$('div').css('left',ev.pageX - disx);
			$('div').css('top',ev.pageY - disy);
		})
		$(document).mouseup(function (ev) {
			$(document).off();
		})
		return false;
	})
})	
_
**12**:
弹窗
*如何创建一个DIV * $('<div>')  这样就是创建
_滚动条事件和窗口调整事件  onresize  onscroll_
**9**:
$() 常用方法
addClass()                       添加样式
removeClass() 				删除样式
width() 						实际 width 没有单位  只能获得原始宽     * windth *
innerWidth() 				    不能获得border 宽                                _windth + padding _
outerWidth() 				可以获得所有属性 总和起来的宽     给个参数true 就能获得 *windth + padding + border + margin*
_ /*---------------------jq中的节点操作-----------------------*/ _
insertBefore() 				节点操作  讲一个点 移动到一个节点前
                              *$('span').insertBefore( $('div') ) ;                  将 div  移动到 span前*
before()
					_ $('span').before( $('div') ) ;   span前面必须是div       也是调换位置_       
inserAfter()
					*$('span').inserAfter( $('div') ); 	将div移动到 span 后*      
after()
					_和  before() 类似_
appendTo()                       添加到一个指定节点的最后
					*$('div').appendTo( $('span') );  这样就将  div 放置在span里最后的位置*
append()
					_和  before() 类似_
prependTo()
					*$('div').prependTo( $('span') );   div 放置在span里开始的位置*
prepend()                    
						_和  before() 类似_        
 _ /*--------------------------------------------*/ _
remove() 		      *删除节点*
on()                              _这个就是事件了   click()  == on('click',function(){})_  更加强大 可以自定义事件 _还可以继续添加其他事件 空格隔开 简直6_
*   				G森形式   (不知道是什么东东)localstorage
							$('div').on(
								'click' : function () {
									alert('123');
								},
								'mouseover' : function () {
									alert('456');
								}
							)
*
$(this).off() 					_这个就是 关闭点击事件     也是可以传参的  关闭指定事件_	
scrollTop()                    *滚动距离*
**5**:
$()下的常用方法
has()           包含意思     $('div').has('span').css('bakcground','red');    //div中包含span 变红
not()              filter 反义词
filter()            过滤
next()              下一个兄弟节点
prev()             上一个兄弟节点
find()           查找的意思
eq()      				eq二联     饿 其实 相当于  js中的下标   
index()          索引      当前元素在所有兄弟节点中的位置
attr()                                 *设置属性  后面会说和其他有什么不同proper deta*
**2**:
*API*
$('li:first').css('background','red');                   *//第一个* first
*$('li:last').css('background','red'); *		//最后一个  last
$('li:eq(2)').css('background','red');     	*//选择第二个*  eq(2)
*$('li:even').css('background','red');*        //奇数行
$('li:odd').css('background','red');       *//偶数行* 
*$('li').filter('.box')*                //过滤				 筛选元素集合中匹配表达式 或 通过传递函数测试的 那些元素集合。
$('li').filter('[title = href]')                   *//筛选出'[title = href]''   title = href的*
*innerHTML == html()*
									*onclick  == click()*
*$(this)*


## mongodb
[mongodb](https://github.com/DemoorBug/comprehensive)

** mongoDB基本使用**
```bash
db.dropDatabase()  删除数据库
show dbs  查看所有表
use imooc  切换数据库 并且自己创建
*在mongo中我们将一个表称作一个集合*
db.imooc_collection.insert({x:1})   插入json格式
show collections  查看表
db.imooc_collection.find()  查询所有
db.imooc_collection.find({x:1})  指定查询
可以使用js语法
for(i=3;i<100;i++) db.imooc_collection.insert({x:i})
db.imooc_collection.find().count() 进行计数
db.imooc_collection.findOne() 

db.imooc_collection.find().skip(3)前三条.limit(2)限制返回2条.sort({x:1}) 使用x排序 1正序 1-倒叙
db.imooc_collection.updateAt({x:1},{x:999})  更新数据
db.imooc_collection.updateAt({x:1},{x:999},true)  更改不存在的数据，自动创建
db.imooc_collection.updateAt({x:1},{$set:{x:999}}) $set操作符为部分操作符
db.imooc_collection.updateAt({x:1},{$set:{x:999}}，false,true) 更新多条数据 第四个参数
db.imooc_collection.remove({c:2})  remove 删除所有数据
db.imooc_collection.drop()  删除表
show tables
*-------------* 
db.imooc_collection.getIndexes()  查看索引
创建成千上万条数据，没有索引将不查找
db.imooc_collection.ensureIndex({x:1})   正项排序      -1 逆向排序   建立索引  单建索引

多建索引与单件索引创建形式相同，区别在于字段的值
	单建索引：值为单一的值，例如字符串，数字或者日期
	多建索引：值具有多个记录，例如数组
*-------------*
db.imooc.ensureIndex({time:1},{expireAfterSeconds:30})     30秒后删除 
db.imooc.insert({time:new Date()})   存储在过期索引字段的值必须是指定的时间类型。说明：必须是ISODate或者ISOData数组，不能使用时间戳，否则不能自动删除
如果指定了ISODate数组，则按照最小的时间进行删除
过期索引不能使复合索引
删除时间不是精确
删除过程是由后台每运60S跑一次，而且删除也需要一些时间，所以存在误差
```

## Node+Express快速搭建电影网站
[Node+Express快速搭建电影网站](https://github.com/DemoorBug/comprehensive/tree/master/%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C)

**Node+Express快速搭建电影网站**
后端是用nodejs来驱动的所以需要：
```
node.js  在这个基础之上的框架  express可以帮我快速的搭建web应用  jade
数据库 mongoDb (芒古DB) 以及对 mongoDb快速建模的工具也就是 mongoose (芒古思) Moment.js
后端的模板引擎 jade (zhei de)
时间和日期的格式化  Moment.js
```
*底下的四个模块都是利用npm安装的   底下的？ 不懂那些*
```
前端
jquery  类库 以及 bootstrap 样式框架  他们都是网站前端的静态资源
网站前端的静态资源呢 都存在一个版本的依赖和版本的管理所以这里我需要通过 Bower(鲍尔) 来安装他们  bower 本身也是一个npm模块 需要npm安装
```
*本地开发环境*
```
后面会用到less编译 样式的合并 语法的检查 包括前后端单元测试的实现 以及服务的自动重启 这几个任务都是通过GRUNT集成 会放到下一期的课程讲  
大概一些模块名字 ：
less cssmin jsHint uglifyJS mocha 不认识 什么鬼
```
**实战开始**
1.需求分析  ```
来看看总共有几个页面 页面都有什么内容 以什么样的交互
```
2.项目依赖初始化 ```
对项目所依赖的模块进行一个安装和初始目录的创建
```
3.入口文件编码  ```
在后端呢创建一个入口文件，来进行编码
```
4.创建视图 ```
创建几个主要页面视图 也就是模板 来跑通前后端的流程 也就是说从浏览器发起一个请求到后端，后端接收到之后呢，再返回一段数据，跑通这个前后端流程以后呢 就可以对页面进行样式的开发 和html Dom结构的填充 同时要为早一些模板的数据 这时候页面基本上都有了 我们现在就开始基于页面里面的内容进行提取抽象同时来设计数据库的模型，然后来开发后端逻辑，到这一步位置前后端的逻辑已经实现，还差一步，就是对前端静态资源版本和后端模块版本进行一个配置文件的生成，然后呢整个网站开发结束
```
*步骤详情全部在4后*
5.测试前端流程
6.样式开发，伪造模板数据
7.设计数据库模型
8.开发后端逻辑
9.配置依赖文件，网站开发结束
**Node入口文件分析**
*项目结构初始化*
- imooc /
	> npm install express
	> npm install jade
	> npm install mongodb
	> npm install bower -g
	> bower install bootstrap
*入口文件编码*
```
var express = require('express');
var app = express()

app.set('view engine','jade');
app.set('port',3000)

app.get('/',function(req,res){
	res.render('index',{title:'imooc'}) 
})
```
_这几行代码大概意思是  引入一个express 模块 生成一个web应用的实例 将这个实例的监听端口设置成3000 然后就可以监听来自这个端口过来的请求_
*创建视图  *
- imooc / 
	- node_modules/    猫丢斯  安装一些模块的时候安装到此目录
	- bower_components/   静态资源 安装到 (鲍尔 康门特) 下
	- views/           v又特  用来测试的主要视图文件
		- index.jade
		- detail.jade
		- admin.jade
		- list.jade

	- app.js   入口文件

*测试前端流程*
- localhost:3000/

- localhost:3000/movie/1

- localhost:3000/admin/movie

- localhost:3000/admin/list
**实际编码**
想创建入口文件  app.js
npm install express jade moment mongoose






**数据库模型设计**
Schema  模式
Model     模型          >       mongoose  >>       mongodb
Documents 文档

模式定义 模式里面呢 对数据字段进行定义
```
var mongoose = require('mongoose')

var MovieSchema = new mongoose.Schema({
	doctor: 'String',
	title: 'String',
	language: 'String',
	country: 'String',
	year: 'Number',
	summary: 'String',
})
```

编译模型  调用mongoose 时对传入schemas 时进行编译然后会生成构造函数
```
var mongoose = require('mongoose');
var MovieSchema = require('../schemas/movie')

var Movie = mongoose.model(
	'movie',
	movieSchema
)
module.exports = Movie

```

文档实例化   只需要调用这个构造函数 传入一条数据  

```
var Movie = require('./models/movie')

var movie = new Movie({
	title : '机械战警',
	doctor : '何塞·帕迪利亚',
	year: 2018
})

movie.save(function(err) {
	if (err) return handleError(err)
})
```
_--_
```
数据库批量查询
var Movie = require('./models/movie')

app.get('/',function(req,res){
	Movie
		.find({})  * 查询全部  *       .findOne({_id:id}) *单条*  
		.exec(function(err,movies){
			res render('index',{
				title:'imooc 首页',
				movies:movies
			})
		})
}

*单条数据删除*
Movie 
	.remove({_id:id}),function(err,movie) {
		if(err) {
			console.log(err)
		}
	}

```

*开发后端逻辑  目录层次调整*

- imooc/
		- node_modules/
		- bower_components/
		- views/
				- \*.jade
		- models/
				- movie.js
		- schemas/
				- movie.js
		app.js

## 定位导航
[定位导航](https://github.com/DemoorBug/comprehensive/tree/master/%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C)

**js编写**
**兼容调试**
*解决IE6不支持问题*
IE6不支持  fixed  要用到 hack  
_解决IE6下跳变的bug_
* html, * html body {
        background-image: url(about:blank);
        background-attachment: fixed;
    }

*html #menu {   <!-- //IE6下生效 -->
	position: absolute;
    top: expression(((e=document.documentElement.scrollTop)?e:document.body.scrollTop)+100+'px');
}

**scroll**
scroll:([data],fn):当用户滚动到指定元素时，会发生scroll事件
_scroll事件适用于所有可滚动的元素和window对象(浏览器窗口)_
*$(window).scroll(function(){/*...*/});*
_scrollTop([val])_  获取/设置匹配元素相应滚动条顶部的偏移
_offset();_ 获取匹配元素的相应偏移。返回的对象包含两个整形属性：top和left，以像素计

## 搜索框
[搜索框](https://github.com/DemoorBug/comprehensive/tree/master/%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C)

**Ajax**
$.get(url,callback,'json'); 
_使用json最大的好处就是不用额外的进行一些编码操作_
*同源策略*
Ajax发生请求的url地址与服务器地址必须是同一域名下
*搭建一个api.bing.com*
使用nginx和fiddler工具设置
**IE兼容**
_< meta http-equiv="X-UA-Compatible" content="IE=edge">_ 防止IE浏览器进入怪异模式 

**标签讲解**
< form action="" target="_blank" method="get">
< /form>
*outline:none* 默认点击后 text 高亮取消


## 画廊笔记
[画廊笔记](https://github.com/DemoorBug/comprehensive/tree/master/%E5%8F%82%E8%80%83%E6%89%8B%E5%86%8C)
**3D效果编写**2016.4.25
transition：property duration timing-function delay
property 规定设置过渡效果的css属性的名称
duration 规定完成过渡效果多少秒或毫秒
timing-function  规定速度效果的速度曲线，默认ease
dalay 定义过渡效果延迟多久开始，默认0
**知识**
-webkit-font-smoothing:antialiased;用于字体平滑
< body onselectstart="return false">
 onselectstart="return false"  防止文字被选中 
-webkit-perspective:800px; // 支持3D
box-sizing: border-box;    // 内容和padd都会在border边框之内去呈现
-webkit-transform-style:preserve-3d;    //支持子元素的3D效果
-webkit-transform:translate(0px,0px) rotateY(0deg);		 			//定义位移以及沿着Y轴旋转
-webkit-backface-visibility:hidden;    当元素不面向屏幕时隐藏

## 放大镜效果
[放大镜效果](https://github.com/DemoorBug/comprehensive)

## 瀑布流布局JQ
[瀑布流布局JQ](https://github.com/DemoorBug/comprehensive)

## 电商网站实战
[电商网站实战](https://github.com/DemoorBug/comprehensive)
