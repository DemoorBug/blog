---
title: vue-qunaerkaifa
date: 2018-09-14 19:53:47
tags: [vue]
categories: [[github项目笔记]]
---

## Vue 去哪儿网站开发
项目地址：[VueLearn -> vue-qunaerkaifa](https://github.com/DemoorBug/VueLearn/tree/master/vue-qunaerkaifa)
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