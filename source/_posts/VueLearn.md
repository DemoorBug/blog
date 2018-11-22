---
title: VueLearn
date: 2018-09-14 19:48:04
tags: [vue]
categories: [github项目笔记]
---

# Vue 入门
项目地址：[VueLearn](https://github.com/DemoorBug/VueLearn)
## 课程大纲
### vue2.5入门教程(vueshili (vue实例) )
>[vue2.5入门教程](https://www.imooc.com/video/16987)

> ?代表存疑问
<!-- more -->

## 总结
### vue2.5
> 用过的记一下

```vue
指令：


v-on:简写 @
v-bind: 简写:
v-for 循环
v-text 纯文本
v-html 字符串转换为代码
v-model 双向数据绑定
对象,实例  ?

el: 貌似是挂载点  ?
data: 数据
methods: 方法
components: 模板
事件：

@click 点击事件
@delete 自定义事件 ?
应该是创建一个'delete'事件 ?
this.$emit('delete',this.index)



@click.native=""          .native 是事件修饰符
```
>安装vue-cli

```
npm install -g vue-cli
vue init webpack {{项目名}}
```

#### 不懂的css
```
box-sizing border-box

display flex
flex-direction column
justify-content center

```

#### 好的css布局
```html
.icons
    height: 0
    padding-bottom: 50%
    overflow: hidden
    background: #ccc
    .icon
      position relative
      overflow hidden
      float: left
      width: 25%
      height 0
      padding-bottom: 25%
      background: red
```

#### 文字超出显示...
```
overflow: hidden
white-space: nowrap
text-overflow: ellipsis
```

#### try catch语法是es6的？
```
let defaultCity = '上海'
try {
  if (localStorage.city) {
    defaultCity = localStorage.city
  }
} catch (e) {}
```

#### 超出能力范围的es6
```
import { mapState } from 'vuex'
export default {
  name: 'HomeHeader',
  computed: {
    ...mapState(['city'])   //不懂
  }
}
```
#### 引入vuex优化代码
```
import { mapState, mapMutations } from 'vuex'

computed: {
...mapState({
  currentCity: 'city'
})
},
methods: {
...mapMutations(['changeCity'])
}


this.$store.state.city
简写为
this.currentCity


this.$store.commit('changeCity', city)
简写为
this.changeCity(city)
```