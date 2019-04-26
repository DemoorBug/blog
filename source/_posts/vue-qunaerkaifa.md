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
push pop shift unshift splice(记录成spilce了，我服了) sort reverse
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
appear 进入动画
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
animate.css提供的动画是@keyframes类型的动画

```html
    <!-- type="transition" 确认动画时长以transition为准 -->
        <transition 
        name="fade"
        type="transition" 
        appear
        enter-active-class="animated zoomInDown"
        leave-active-class="animated zoomIn"
        appear-active-class="animated zoomInLeft"
        >

```

通过
type
:duration 属性可以控制动画总时长
存在即合理
```html
    <style>
        .fade-enter,
        .fade-leave-to {
            opacity: 0;
        }

        .fade-enter-active,
        .fade-leave-active {
            transition: all 10s;
        }
        .nams {
            width: 100px;
            height: 100px;
            text-align: center;
            line-height: 100px;
        }
    </style>
    <div id="root">
        <!-- type="transition" 确认动画时长以transition为准 -->
        <transition
        :duration="{enter: 5000, leave: 10000}"
        type="transition"
        name="fade"
        appear
        enter-active-class="animated zoomInDown fade-enter-active"
        leave-active-class="animated zoomIn fade-leave-active"
        appear-active-class="animated zoomInLeft"
        >  <!-- 使用animate动画 --> <!-- 自定义class  appear -->
            <div v-show="show" class="nams">hello world</div>
        </transition>
        <button @click="handclick">切换</button>
    </div>

```
使用type属性需要注意transition必须存在，不然动画就==没有时长了

## 5-4Vue中的js动画与Velocity.js结合

**一，js钩子实现动画**

```
    <div id="root">
        <transition
        @before-enter="handleBeforeEnter"
        @enter="handleEnter"
        @after-enter="handAfterEnter"
        >  <!-- 出场动画就是将enter改为leave -->
            <div v-show="show" class="nams">hello world</div>
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
                },
                handleBeforeEnter: function(el) {
                     el.style.color = 'red'
                },
                handleEnter: function(el,done) { /*done回调函数*/
                    setTimeout(() => {
                        el.style.color = 'green'
                    },2000)
                    setTimeout(() => {
                        done()   /*这里调用done()触发@after-enter事件*/
                    },4000)
                },
                handAfterEnter: function(el){
                    el.style.color = 'black'
                }
            }
        })
    </script>
```

**二、velocity.js实现动画**
[velocity官网](http://velocityjs.org/)

样式部分没有变
```
    <script src="velocity.min.js"></script>

    <script>
        var vm = new Vue({
            el: '#root',
            data: {
                show: true
            },
            methods: {
                handclick: function() {
                    this.show = !this.show
                },
                handleBeforeEnter: function(el) {
                    el.style.opacity = 0;
                },
                handleEnter: function(el,done) { /*done回调函数*/
                    Velocity(el, {
                        opacity: 1
                    },{
                        duration: 1000,
                        complete: done   <!-- 当Velocity执行完动画后 后面内容会被自动执行 -->
                    })
                },
                handAfterEnter: function(el){
                    alert('动画结束')
                }
            }
        })
    </script>

```

## Vue中多个元素或组件的过渡
一、多个元素中的过渡效果
因为vue复用组件的关系，所以要加key
mode可以控制动画样式，in-out先显示再隐藏，out-in隐藏再显示
```html
    <style>
        .v-enter , .v-leave-to{
            opacity: 0
        }
        .v-enter-active, .v-leave-active {
            transition: opacity 1s;
        }
    </style>
    <div id="root">
        <transition
        mode="out-in"
        > <!-- mode可以控制动画样式，in-out先显示再隐藏，out-in隐藏再显示 -->
            <div v-if="show" class="nams" key="hello">hello world</div>
            <div v-else key="bye">Bye World change Account login</div>
        </transition>
        <button @click="handclick">切换</button>
    </div>

```
二、多个组件间的过渡

动态组件实现过渡效果
<component></component> 动态组件

```html
    <div id="root">
        <transition
        mode="out-in"
        > <!-- mode可以控制动画样式，in-out先显示再隐藏，out-in隐藏再显示 -->
            <component :is="type"></component> <!-- 动态组件 -->
        </transition>
        <button @click="handclick">切换</button>
    </div>

    <script>
        Vue.component('child', {
            template: '<div>child</div>'
        })
        Vue.component('child-on', {
            template: '<div>child-on</div>'
        })

        var vm = new Vue({
            el: '#root',
            data: {
                type: 'child'
            },
            methods: {
                handclick: function() {
                    this.type = this.type === 'child' ? 'child-on' : 'child'
                },

            }
        })
    </script>
```

## 5-6、Vue中的列表过渡
<transition-group></transition-group>相当于给每一个标签加了一层<transition></transition>
自己加了个删除，还不错
```html
    <style>
        .v-enter , .v-leave-to {
            opacity: 0;
        }
        .v-enter-active, .v-leave-active {
            transition: opacity 1s;
        }
    </style>
    <div id="root">
        <transition-group
        >
            <div v-for="item of list" :key="item.id">
                {{item.title}}
                {{item.id}} 
            </div>
        </transition-group>
        <button @click="handclick">Add</button>
        <button @click="remos">remove</button>
    </div>

    <script>
        var count = 0;
        var vm = new Vue({
            el: '#root',
            data: {
                list: []
            },
            methods: {
                handclick: function() {
                    this.list.push({
                        id: count ++,
                        title: 'modelist'
                    })
                },
                remos: function() {
                    console.log(this.list.length)
                    this.list.splice(this.list.length-1,1)
                }
            }
        })
    </script>


```
## 5-7 Vue中的动画封装
自己用了下velocity.js动画
*封装动画为什么不能用v-show,搞了半天原来是这个问题，我还以为老师代码有错，插入的内容我没有去v-show,<div v-show="show">hello world
            </div>写成这样了，怪不得错呢*
```html
    <script src="velocity.min.js"></script>

    <div id="root">
        <fade :show="show"
        >
            <div>hello world
            </div>
        </fade>
        <button @click="handclick">mos</button>
    </div>

    <script>
        Vue.component('fade', {
            props: ['show'],
            template: `
                <transition @before-enter="handleBeforeEnter" @enter="handleEnter">
                    <slot v-if="show"></slot>
                </transition>
            `,
            methods: {
                handleBeforeEnter: function(el) {
                    el.style.opacity = 0;
                    console.log(1)
                },
                handleEnter: function(el,done) {
                    console.log(2)
                    Velocity(el, {
                        opacity: 1,
                    },{
                        duration: 1000,
                        complete: done
                    })
                }

            }
        })
        var vm = new Vue({
            el: '#root',
            data: {
                show: false
            },
            methods: {
                handclick: function() {
                    this.show = !this.show
                }
            }
            
        })
    </script>
```

## 5-8 章节小节
第5节主要讲解了过渡动画
@keyframes动画
在vue中通过js如何实现动画
同时我们简单的说了下vue和css或者Velocity.js这样的动画库
多个元素切换过程中的动画  
最后是列表动画的内容

**课后作业**
可以参考官方的文档，来完成作业
动态过度
状态过度

一、动态过渡
**vue props接收值用data替换,props值改变data值不变？**所遇到的问题，还没有解决
百度到的答案，深度拷贝
[google到的，明天看吧](https://blog.csdn.net/o_Mario_o/article/details/77035451)

这个代码虽然实现了，但是还有问题，就是props接收的值我直接修改了
**这个坑留这里把，看看教程后面有解决方法没**
```html
    <script src="velocity.min.js"></script>

    <div id="root">
        <input type="range" min="0" max="2000" model="500" v-model="indexnames1">
        <input type="range" min="0" max="2000" model="500" v-model="indexnames2">
        <fade :show="showa" :index1="indexnames1" :index2="indexnames2"
        >
            <div>hello world
            </div>
        </fade>
        <button @click="handclick">mos</button>
    </div>

    <script>
        Vue.component('fade', {
            props: ['show','index1','index2'],
            template: `
                <transition @before-enter="handleBeforeEnter" @enter="handleEnter" @leave="hanleave">
                    <slot v-if="show"></slot>
                </transition>
            `,
            methods: {
                handleBeforeEnter: function(el) {
                    el.style.opacity = 0;
                },
                handleEnter: function(el,done) {
                    var this_ = this
                    Velocity(el, {
                        opacity: 1,
                    },{
                        duration: this_.index1,
                        complete: function() {
                            done()
                            this_.show= !this_.show
                        }
                    })
                },
                hanleave: function(el,done) {
                    var this_ = this
                    Velocity(el, {
                        opacity: 0
                    },{
                        duration: this_.index2,
                        complete: function() {
                            done()
                            this_.show= !this_.show
                        }
                    })
                }

            }
        })
        var vm = new Vue({
            el: '#root',
            data: {
                showa: false,
                indexnames1: '1000',
                indexnames2: '1000'
            },
            methods: {
                handclick: function() {
                    cas: '2'
                    this.showa = !this.showa
                }
            }
            
        })
    </script>
```

填坑：
用watch监听传入的变化，然后赋值给一个新值，修改此值就可以了，我的天啊，好简单啊，深度拷贝完全不需要，我想多了
```js
Vue.component('fade', {
    props: ['show','index1','index2'],
    data () {
      return {
        hide: this.show
      }
    },
    template: `
        <transition @before-enter="handleBeforeEnter" @enter="handleEnter" @leave="hanleave">
            <slot v-if="hide"></slot>
        </transition>
    `,
    computed: {
      // shows: function () {
      //   return this.show
      // }
    },
    watch: {
      show: function () {
        this.hide = this.show
      }
    },
    methods: {
        handleBeforeEnter: function(el) {
            el.style.opacity = 0;
        },
        handleEnter: function(el,done) {
            var this_ = this
            Velocity(el, {
                opacity: 1,
            },{
                duration: this_.index1,
                complete: function() {
                    done()
                    this_.hide = !this_.hide

                }
            })
        },
        hanleave: function(el,done) {
            var this_ = this
            Velocity(el, {
                opacity: 0
            },{
                duration: this_.index2,
                complete: function() {
                    done()
                    this_.hide = !this_.hide
                }
            })
        }

    }
})
var vm = new Vue({
    el: '#root',
    data: {
        showa: false,
        indexnames1: '1000',
        indexnames2: '1000'
    },
    methods: {
        handclick: function() {
            cas: '2'
            this.showa = !this.showa
        }
    }
    
})
```
## 6-1 项目环境准备
安装node什么的，我自然都会了，还有git，我用的是桌面端，有机会用用字符的

```html
npm install -g vue-cli
vue init webpack vue
cd vue
npm install
npm run dev

```

## 6-2项目代码结构介绍
项目地址：[VueLearn -> VueQuNaEr](https://github.com/DemoorBug/VueLearn/tree/master/VueQuNaEr)
-   README.md 项目初始化文件
-   package.json 项目依赖
-   package-lock.json 一个锁文件，可以确定安装第三方包的具体版本，保持团队编程的统一
-   index.html 项目默认首页模板文件
-   .gitignore  禁止git上传文件
-   .eslintrc.js js代码规范，必须按照这个规范写
-   .eslintignore 这里面的目录及文件不会受到eslint规范影响
-   .editorconfig 规范书写代码
-   .babeirc 写ES6语法，转换为浏览器可以解析的代码，这个是配置文件
-   static 存放静态资源，图片，模拟json数据
-   node_modules 
-   src 整个项目源代码
    -   main.js项目入口文件
    -   App.vue 最原始的根组件
    -   router 路由
    -   components 组件
    -   assets 图片类资源
-   config 文件夹下放的是项目配置文件
    -   index.js 基础配置信息
    -   dev.env.js 开发环境配置信息
    -   prod.env.js 线上环境配置信息
-    build 项目打包webpack配置内容，一般来说不需要修改
**一般来说不需要对这些文件修改，我们要做的就是在src源代码目录下进行我们业务代码的开发**
## 6-3 单文件组件与Vue中的路由

// 路由就是根据网址的不同，返回不同的内容给用户
**路由这章没什么好说的，angular的时候就已经很熟了**

## 6-4 多页应用VS单页应用
代替a
 <router-link to="/list">列表页</router-link>

服务器渲染可以完美解决单页面中的问题
首屏时间稍慢，SEO差

## 6-5 项目代码初始化

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum-scale=1.0,user-scalable=no">
```
添加了不允许用户放大缩小的代码
-   移动端重置代码引入reset.css文件
-   移动端1像素边框问题引入border.css文件
-   移动端延迟300秒点击延迟问题
    -   安装第三方模块 
    -   npm install fastclick --save
    -   import fastClick from 'fastclick'
    -   fastClick.attach(document.body)
-   iconfont
上传代码git 
```bash
git add . 
git commit -m 'project init '
git push
```

## 7-1 首页header区域开发
四百勒斯类似less，cass
安装stylus
```bash
npm install stylus --save
npm install stylus-loader --save
```
-   home拆分成很多部分，在script里面引入就可以了
    -   import HomeHeader from './components/Header'
-   样式部分用stylus写，而且是不影响全局作用域
    -   <style lang="stylus" scoped></style>
-   移动端采用rem布局，rem是根据html设置的font-size规定的大小
-   引入全局的stylus文件，通用样式可以放到这里面，就可以复用了
-   @import '~styles/varibles' 在style中使用的技巧，加~符号，不然会报错
-   创建别名webpack.base.conf.js
    -   'styles': resolve('src/assets/styles')
-   修改webpack配置项的时候，一定要重启服务
**display: flex这个属性很好用，以后应该会经常遇到**

## 7-3首页轮播图
### 分支
```bash
git pull   线上的分支拉到本地
git checkout index-swiper   切换分支
git status 查看分支
```
### 轮播插件
[vue-awesome-swiper](https://github.com/surmon-china/vue-awesome-swiper)
```bash
npm install vue-awesome-swiper@2.6.7 --save
```
### 全局引入
```html
import Vue from 'vue'
import VueAwesomeSwiper from 'vue-awesome-swiper'

// require styles
import 'swiper/dist/css/swiper.css'

Vue.use(VueAwesomeSwiper, /* { default global options } */)
```
### 不懂的css
```css
    width: 100%
    height: 31.25vw   == 31.25%相当于宽的
```
### 兼容性更高的写法
```css
    width: 100%
    height: 0
    overflow: hidden
    padding-bottom: 31.25%  这个比例算法有问题，莫名其妙
```
### 鼠标移动到下方5-10像素距离还是可以拖动
[解决方法参考这个文章，遇到的问题里面也有描述](https://github.com/surmon-china/vue-awesome-swiper/issues/423)
大概就是给html元素添加
```css
touch-action: none;
```
### 样式穿透
在有scope作用域的style下可以这样给其他页面应用样式
```css
.wrapper >>> .swiper-pagination-bullet-active
    background: #fff
```
添加  >>>
### git提交以及合并
```bash
git add
git commit -m 'change'
git push
git checkout master          #切换分支
git merge origin/index-swiper  #线上内容合并
git push 
```
## 7-4 图标区域页面布局
### github线上创建分支，拉过来
```
git pull
git checkout index-icons

```
## 7-5 图标区域逻辑实现
### js真滴神奇，

```
    pages () {
      const pages = []
      this.iconList.forEach((item, index) => {
        const page = Math.floor(index / 8)
        if (!pages[page]) {
          pages[page] = []
        }
        pages[page].push(item)
      })
      return pages
    }
```
## 7-6 推荐组件开发
### 本地创建分支

```bash
git checkout -b index-recommend //创建并切换到newbranch分支下

git push origin index-recommend //推送到远程仓库的newbranch分支下，没有就创建
```
> 他是下面命令的简写

```bash
git branch index-recommend
git checkout index-recommend
```
### 没什么讲究的，就是一个
```html
    .item-info
      flex 1
      padding .1rem
      min-width 0
```
## 7-7 周末游组件开发


## Ajax获取首页数据
### 老师切换分支的时候发现一个本地错误
```
git status
git checkout . 去除更改
git status 这次查看本地分支和线上分支一致

```
### vue中使用ajax
-   fech 浏览器自带函数
-   vue-resource
-   官方推荐axios跨平台请求
    -   浏览器端可以帮你发送shr请求
    -   node端可以帮你发送http请求

### 使用axios
```
npm install axios --save
```
### 开发环境转发
Paths这个功能是webpack-dev-server提供的
> config/index.js
```
    proxyTable: {
      '/api': {
        target: 'http://localhost:8080',
        pathRewrite: {
          '^/api': '/static/mock'
        }
      }
    }
```
改变配置文件需要重启

## 首页父子组件数据传递
```
methods: {
    getHomeInfo () {
      axios.get('/api/index.json')
        .then(this.getHomeInfoSucc)
    },
    getHomeInfoSucc (res) {
      res = res.data
      if (res.ret && res.data) {
        const data = res.data
        this.city = data.city
        this.swiperList = data.swiperList
        this.iconList = data.iconList
        this.recommendList = data.recommendList
        this.weekendList = data.WeekendList
      }
    }
  },
  mounted () {
    this.getHomeInfo()
  }
```

## 8-1城市选择页面路由配置
页面a链接替换组件
```html
<router-link to='/city'></router-link>
```

## 8-2搜索框布局


## 8-3列表布局

## 8-4Better-scroll的使用及字母表布局
安装
```
npm install better-scroll --save
```

## 8-5页面动态数据渲染


## 8-6兄弟组件联动
```
this.scroll.srollToElement()
```
首先确定自己的位置
获得a字母距离顶部高度
滑动的时候确定手指位置距离顶部高度
做一个差值就能够算出当前手指位置和a顶部的差值，再除以每个字母的高度就可以知道当前是第几个字母了

```js
        this.startY = this.$refs['A'][0].offsetTop
        const touchY = e.touches[0].clientY - 79
        const index = Math.floor((touchY - this.startY) / 20)
        if (index >= 0 && index < this.letters.length) {
          this.$emit('change', this.letters[index])
        }
```
生命周期钩子
```
updated () {
    this.startY = this.$refs['A'][0].offsetTop
  }
```
## 8-7列表切换性能优化

函数截留
```
    timer: null

    if (this.timer) {
      clearTimeout(this.timer)
    }
    this.timer = setTimeout(() => {},16)
```

## 8-8s搜索功能实现
```javascript
import Bscroll from 'better-scroll'
export default {
  name: 'CitySearch',
  props: {
    cities: Object
  },
  data () {
    return {
      keyword: '',
      list: [],
      timer: null
    }
  },
  computed: {
    hasNoData () {
      return !this.list.length
    }
  },
  watch: {
    keyword () {
      if (this.timer) {
        clearTimeout(this.timer)
      }
      if (!this.keyword) {
        this.list = []
        return
      }
      this.timer = setTimeout(() => {
        const result = []
        for (let i in this.cities) {
          this.cities[i].forEach((value) => {
            if (value.spell.indexOf(this.keyword) > -1 || value.name.indexOf(this.keyword) > -1) {
              result.push(value)
            }
          })
        }
        this.list = result
      }, 100)
    }
  },
  mounted () {
    this.scroll = new Bscroll(this.$refs.search)
  }
}
```

## 8-9使用vuex实现数据共享
[vuex](https://vuex.vuejs.org/zh-cn/intro.html)

## Vuex的高级使用及localStorage
一些复杂的可以简写要引入vuex模块
```js
import { mapState, mapMutations } from 'vuex'
export default {
  computed: {
    ...mapState({
      currentCity: 'city'
    })
  },
  methods: {
    handclick (city) {
      // this.$store.commit('changeCity', city)
      this.changeCity(city)
      this.$router.push('/')
    },
    ...mapMutations(['changeCity'])
  }
}
```

## 使用keep-alive 优化网页性能
```
<keep-alive></keep-alive>
```
将内容放到内存中，就不会出现重复请求ajax了

```
mounted () {
    this.lastCity = this.city
    this.getHomeInfo()
  },
  activated () {
    if (this.lastCity !== this.city) {
      this.lastCity = this.city
      this.getHomeInfo()
    }
  }
```
当有keep-alive的话就会多一个生命周期钩子，activated
这个钩子会在页面重新出现的时候执行
> 原来是ok的，老师的逻辑没问题，我自己搞错了


## 详情页动态路由及banner布局

## 公用图片画廊组件拆分

## 9-3实现Header渐隐渐显效果
js绑定事件。。。好久没用过了
```
activated () {
    window.addEventListener('scroll', this.handleScroll)
  }
```
js动画+限制最大值
```
handleScroll () {
      const top = document.documentElement.scrollTop
      if (top > 60) {
        let opacity = top / 140    //动画原理
        opacity = opacity > 1 ? 1 : opacity  //限制最大值
        this.opacityStyle = {
          opacity
        }
        this.showAbs = false
      } else {
        this.showAbs = true
      }
    }
  },
```

## 9-4对全局事件的解绑
这个生命周期钩子，在页面关闭时执行
```
deactivated () {}
```
## 9-5 使用递归组件实现详情页列表
自己调用自己，太6了
原来name主要作用就是递归组件自己调用自己的时候使用


## 9-6使用ajax获取动态数据
```
<template>
  <div id="app">
    <keep-alive exclude="Detail">
      <router-view/>
    </keep-alive>
  </div>
</template>
```

```
exclude="Detail"
```
不缓存Detail组件的数据

name到底什么作用，使用递归组件会用到，相对某个页面取消换成可以用到，vue开发组件可以获得name名字

每次进行路由切换显示都会初始化
```
scrollBehavior (to, from, savedPosition) {
    return { x:0, y:0 }
  }
```

## 在项目中加入基础动画
animation动画

## vue项目的接口联调
