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