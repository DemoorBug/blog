---
title: vue-qunaerkaifa
date: 2018-09-14 19:53:47
tags: [vue]
categories: [[github项目笔记]]
---

## Vue 去哪儿网站开发
项目地址：[VueLearn -> vue-qunaerkaifa](https://github.com/DemoorBug/VueLearn/tree/master/vue-qunaerkaifa)
Vue官方文档：老师都是和官方文档一起讲解的，[vue官方文档](https://cn.vuejs.org/v2/guide/)
## mvvm开发模式2-4
```
平常我们用的是mvp开发模式   Model 数据ajax presenter 控制器 View 视图

MVVM 我们开发的话只用关心MV这两个层面   VM是VUE帮我们完成的
M层数据 
V层视图  
VM层做的事情就是当M层数据改变去更改V层，这个都是VUE帮我们完成的

```
<!-- more -->
>虚拟DOM 还有个什么拼不出来，老师让自己翻阅资料查看

## 组件化2-5
```
页面本来是一个整体，组件化就是把它们切分为一个一个部分， 每一个部分都称之为一个组件
更容易维护

```

## Todolist组件化2-6
```js
全局模板-组件

Vue.component('TodoItem',{                   创建全局模板
	props: ['content'],        {'item':content}                     接收
	template: '<li>{{content}}</li>',
})
使用：上面创建的时候使用了驼峰，所以下面可以这样使用 还真是智能
<todo-item></todo-item>

局部模板-组件

var TodoItem = {
	props: ['content'], 
	template: '<li>{{content}}</li>',
}

var app = new Vue({
	components: {
		TodoItem: TodoItem
	}
})

```
## 组件间传值2-7

```js
this.$emit('delete')  
子组件传递事件，这个不但可以传递事件，还可以传递参数，我估计直接传递一个对象都可以吧
this.$emit('delete',this.index)

父组件用@delete='' 绑定监听事件

.splice(index,1) 删除数组内容

```

## 3-1

```
vm = new Vue({

	})
vm.$el 

vm.$data

凡是以$符号开头的都是指vue的实例属性，或者实例方法

vm.$destroy()  销毁vue实例


```

## vue生命周期


## 3-4计算属性，方法，侦听器 

```
计算属性 内置缓存 依赖的值没有改变就不会刷新，高效
computed : {}

methods

侦听器：当这个数据改变了就会执行对应的函数，也具有缓存机制
watch: {

}

```

## 计算属性的setter和getter  3-5

```
coputed: {
	fullName: {
		get: function() {
			return
			},
		set: function(value) {    value值是get的返回值
			console.log(value)
		}
	}
}

更改set表示设置了fullName就会更改set值

```
## 样式绑定

```
类绑定
<div :class="{class: true}"></div>
<div :class="className"></div>
<div :class="[ClassName,{class: true}]"></div>
styel绑定

style绑定
<div :style="{fontSize: '20px'}"></div>
<div :style="[styleName,{fontSize: '20px'}]"></div>

需要注意 style需要试用驼峰式写法
```

这里是样式绑定这一节的代码块，
```vue
	<style>
		.activated {
			color: red;
		}
	</style>
	<div id="app">
		<div @Click="handleClick"
			 :class="{activated: isActivated}">Hell world</div>
	</div>

	<script>
		var vm = new Vue({
			el: "#app",
			data: {
				isActivated: false
			},
			methods: {
				handleClick: function(){
					this.isActivated = !this.isActivated
				}
			}
		})
	</script>
```

三元表达式
```
	普通写法：
	if(this.activated === "activated"){
		this.activated= "";
	}else {
		this.activated= "activated;
	}

 this.activated= this.activated === "activated" ? "" : "activated"
```

## 3-7 Vue中的条件渲染
通过v-if 指令结合js表达式的返回值来决定一个标签是否在页面上显示或者是觉得这个标签是否在页面上存在

```vue
v-show 增加display：none属性 性能高√
v-if 从DOM结构移除 
v-else   要和v-if紧贴使用
v-else-if=""
key  当一个元素被加key值后，vue会知道他是页面上唯一一个值，如果两个东西(应该没有同样的key值吧)的key值不一样，vue就不会去复用以前的dom
```

## 3-8 Vue中的列表渲染

```
<div id="app">
		<div v-for="(item, index) of list" :key="item.id">
			{{item.text}} ---- {{index}}
		</div>
	</div>

	<script>
		var vm = new Vue({
			el: "#app",
			data: {
				list: [{
					id: "01",
					text: "hello"
				},{
					id: "02",
					text: "blue"
				},{
					id: "03",
					text: "less"
				}]
			}
		})
	</script>
```
item 得到的数据，index下标，of是什么东西(以前用的in)，list数组

key值不要使用index，使用项目中自带的id，

这几个变异(？)方法可以达到数据变内容变
push pop shift unshift spilce sort reverse
第二种方法是改变数据的引用地址，从而改变内容

对象列表有3个值v-for="(item, key, index) of info",
item 对象里面的键名，key键值， index位置信息，下标

## 3-9Vue中的set方法
通过set方法，注入数据，页面也会跟着变
对象中的set方法:
```vue
Vue.set(vm.userinfo,"address","beijing")
vm.$set(vm.userinfo,"address","beijing")
```
数组中set方法使用:
Vue.set(vm.userinfo,1,5)  1是下标
vm.$set(vm.userInfo,2,10)

**想改变页面上内容数组有三种方法(改变引用，变异方法，set方法)，对象有两种方法(改变引用，set方法)**

## 4-1组件使用的细节点

一、因为html5规范，tbody里面只能放tr，所以用vue模板的时候要这样写
```
<tr is="row"></tr>
```
使用is属性解决模板bug

二、子组件定义data，后面必须是一个函数(function),之所以这么设计，是因为一个子组件不像根组件那样只被调用一次，它可能在不同的地方被调用
```
data: function() {
	return {
		content: 'this is content'
	}
},
```
三、通过ref='hello'获取dom节点，达成更改dom的操作
```
methods: {
	handleClick: function() {
	this.$refs.hello.innerHTML
}
}
```
当ref在一个标签上的时候通过this.$refs.名字获取到的内容实际上是获取到的dom节点
当ref在一个组件上的时候通过this.$refs.名字获取到里面的内容的时候你获取到的实际上是子组件的**引用**
本节的代码。。
```html
	<div id="root">
		<row @change="changeClick" ref='woder'></row>
		<row @change="changeClick" ref='hello'></row>
		<div>{{numberAll}}</div>
	</div>

	<script>
		Vue.component('row',{
			template: '<div @click="handleClick">{{number}}</div>',
			data: function(){
				return {
					number: 0
				}
			},
			methods: {
				handleClick: function(){
					this.number ++
					this.$emit('change')
				}
			}

		})
		var vm = new Vue({
			el: "#root",
			data: {
				numberAll: 0
			},
			methods: {
				changeClick: function(){
					this.numberAll = this.$refs.hello.number + this.$refs.woder.number
				}
			}
		})
	</script>
```
## 4-2父子组件的数据传递 
更多的传递方式，上一节只是一种
一丶父组件通过属性的方式传递数据
```vue
	<counter :count="0"></counter>

	var counter = {
		props: ['count'],
		template: '<div @click="handleClick">{{count}}</div>',
		methods: {
			handleClick: function(){
				this.count ++          这里不能直接修改
			}
		}
	}
```
单项数据流，子组件不能直接修改父组件参数
```vue
	var counter = {
		props: ['count'],
		data: function(){
			return {
				number: this.count
			}
		},
		template: '<div @click="handleClick">{{number}}</div>',
		methods: {
			handleClick: function(){
				this.number ++          
			}
		}
	}

```
本节代码:
```html
	<div id="root">
		<counter :count="0" @change='char'></counter>
		<counter :count="1" @change='char'></counter> 
		<div>{{chas}}</div>
	</div>

	<script>
		var counter = {
			props: ['count'],
			data: function(){
				return {
					number: this.count
				}
			},
			template: '<div @click="handleClick">{{number}}</div>',
			methods: {
				handleClick: function(){
					this.number ++ 
					this.$emit('change', 1)         
				}
			}
		}

		var vm = new Vue({
			el: '#root',
			components: {
				counter: counter
			},
			data: {
				chas: 1
			},
			methods: {
				char: function(step){
					this.chas += step
				}

			}
		})
	</script>
```

## 4-3组件参数效验于非props特性
传递参数的时候，:content="这里是差值表达式，自然就是Number类型"
content="这样就是字符串"
**一丶props可以是一个对象，规定接收的是什么类型的值(可以接收多个值)**
```html
		Vue.component('child',{
			props: {
				content: [String, Number]
			},
			template: '<div>{{content}}</div>'
		})
```
还可以更复杂props,还可以跟一个对象
```html
		Vue.component('child',{
			props: {
				content: {
					type: String,
					required: true,  //content不能没有
					default: 'default value',  //默认值
					validator: function(value) {
						return (value.length > 5)
					}  //规定长度
				}
			},
			template: '<div>{{content}}</div>'
		})
```
非props个人理解，父组件传，子组件不接收，子组件就不能用这个参数(那不是p话？)，参数会显示到标签上
非props特性，是什么鬼呦

## 4-4给组件绑定原生事件
原生事件就是组件本事绑定事件，组件拿出来用给的事件就是自定义事件
```html
	<div id="root">
		<child @click='handleClick'></child>   //自定义事件
	</div>

	<script>
		Vue.component('child',{
			template: '<div @click="handbas">child</div>',  //原生事件
			methods: {
				handbas: function(){
					alert('handbas')
					this.$emit('click')
				}
			}
		})
		var vm = new Vue({
			el: '#root',
			methods: {
				handleClick: function() {
					alert('click')
				}
			}
		})
	</script>
 ```
 但是上面的需要两层传递，很麻烦，还有下面的方法
 ```html
	<div id="root">
		<child @click.native='handleClick'></child>
	</div>

	<script>
		Vue.component('child',{
			template: '<div>child</div>',
		})
		var vm = new Vue({
			el: '#root',
			methods: {
				handleClick: function() {
					alert('click')
				}
			}
		})
	</script>
 ```

 总结：绑定.native修饰符就可以了

 ## 4-5非父子组件间的传值(Bus/总线/发布订阅模式/观察者模式)
Vuex
 ```
	<div id="root">
		<child content='Dell'></child>
		<child content='Lee'></child>
  	</div>

	<script>
		Vue.prototype.bus = new Vue()  //prototype

		Vue.component('child',{
			props: {
				content: String
			},
			data: function() {
				return {
					number: this.content
				}
			},
			template: '<div @click="handleClick">{{number}}</div>',
			methods: {
				handleClick: function() {
					this.bus.$emit('change', this.number)    //this.bus
				}
			},
			mounted: function() {    //
				var this_ = this
				this.bus.$on('change', function(msg){
					this_.number = msg
				})
			}
		})

		var vm = new Vue({
			el: '#root',
			methods: {
				hands: function(step) {

				}
			}
		})

	</script>

 ```

4-6 在Vue中使用插槽(slot)
**一、插槽**

```html
	<div id="root">
		<child>
			<p>Dell</p>  插槽
		</child>
  	</div>

	<script>
		
		Vue.component('child',{
			props: {
				content: String
			},
			template: `<div>
							<p>hello</p>
							<slot>默认内容</slot>  //slot语法接收，当没有内容的时候就会使用默认内容
					   </div>`
		})

		var vm = new Vue({
			el: '#root',
		})

	</script>

```

**二、句名插槽**
```html
	<div id="root">
		<body-content>
			<div class="header" slot='header'>header</div>
			<div class="footer" slot='footer'>footer</div>
		</body-content>
  	</div>

	<script>
		
		Vue.component('bodyContent',{
			props: {
				content: String
			},
			template: `<div>
							<slot name='header'></slot>
							<div class="content">content</div>
							<slot name='footer'></slot>
					   </div>`
		})

		var vm = new Vue({
			el: '#root',
		})

```

4-7 Vue中的作用域插槽

```html
	<div id="root">
		<child>
			<template slot-scope="props">   <!-- 固定写法 -->
				<li>{{props.item}}</li>
			</template>
		</child>
  	</div>

	<script>
		
		Vue.component('child',{
			data: function() {
				return {
					list: [1,2,3,4]
				}
			},
			template: `<div>
							<ul>
								<slot v-for="item of list" :item="item"></slot>
							</ul>
						</div>`
		})

		var vm = new Vue({
			el: '#root',
		})

	</script>
```

4-8动态组件与v-once指令
**一、普通写法，实现(toggle)互相展示隐藏的效果**
```html
	<div id="root">
		<child-one v-if='type === "child-one"'></child-one>
		<child-two v-if='type === "child-two"'></child-two>
		<button @click="handClik">change</button>
  	</div>

	<script>
		
		Vue.component('childOne',{
			template: '<div>child-one</div>'
		})

		Vue.component('childTwo',{
			template: '<div>child-two</div>'
		})

		var vm = new Vue({
			el: '#root',
			data: {
				type: 'child-one'
			},
			methods: {
				handClik: function() {
					this.type = this.type === 'child-one' ? 'child-two' : 'child-one'
				}
			}
		})

	</script>
```
**二、动态组件**
script部分就是上面的
```html
	<div id="root">
		<component :is='type'></component>  <!-- 动态组件 -->
		<button @click="handClik">change</button>
  	</div>
```
**三、v-once指令**
保存到内存，高效
```html
		Vue.component('childOne',{
			template: '<div v-once>child-one</div>'
		})

		Vue.component('childTwo',{
			template: '<div v-once >child-two</div>'
		})
```
v-once提高静态资源的效率

## Vue中的css动画原理

transition标签，工作原理
显示标签的时候enter
即将运行的第一帧就存在了class： 
fade-enter  
fade-enter-active
运行到第二帧class：
删除fade-enter
fade-enter-to
最后就是删除所有帧

删除标签的时候leave，原理和上面一样

```html
如果没有name="fade" class名字就是v-enter以此类推

<transition name="fade">  <!-- 包裹的内容有过渡效果 -->
</transition>

```
v-if,
v-show　　都可以实现
本节源码
```html
	<style>
		.fade-enter,
		.fade-leave-to {
			opacity: 0;
		}
		.fade-enter-active,
		.fade-leave-active {
			transition: opacity 1s;
		}
	</style>

	<div id="root">
		<transition name="fade">  <!-- 包裹的内容有过渡效果 -->
			<div v-if="show">hello world</div>
		</transition>
		<button @click="handclick">切换</button>
  	</div>

	<script>
		var vm = new Vue({
			el: '#root',
			data: {
				show: true
			},
			methods: {
				handclick: function() {
					this.show = !this.show
				}
			}
		})
	</script>

```

## 在Vue中使用Animate.css库
enter-active-class  可以替换 v-enter-active这个class，所以我们可以利用这个特性使用Animate库

```
<link rel="stylesheet" href="node_modules/animate.css/animate.css">

	<div id="root">
		<transition 
		name="fade"
		enter-active-class="animated bounce"
		leave-active-class="animated bounce"
		>  <!-- 使用animate动画 -->
			<div v-if="show">hello world</div>
		</transition>
		<button @click="handclick">切换</button>
  	</div>


	<script>
		var vm = new Vue({
			el: '#root',
			data: {
				show: true
			},
			methods: {
				handclick: function() {
					this.show = !this.show
				}
			}
		})
	</script>
```

**需要注意的是自定义class，animated添加这个class，根据喜欢的**

## 5-3在Vue中同时使用过度和动画

```html
	<div id="root">
		<transition 
		name="fade"
		appear
		enter-active-class="animated zoomInDown"
		leave-active-class="animated zoomIn"
		appear-active-class="animated zoomInLeft"
		>  <!-- 使用animate动画 --> <!-- 自定义class  appear -->
			<div v-show="show" class="nams">hello world</div>
		</transition>
		<button @click="handclick">切换</button>
  	</div>
```