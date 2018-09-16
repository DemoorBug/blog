---
title: angular
date: 2018-09-14 23:52:49
tags: [angular]
categories: [github项目笔记]
---

# MVC 只是手段，终极目标是模块化和复用
项目地址：[angular](https://github.com/DemoorBug/angular)
|时间|简介|内容|
|--|--|--|
|9-13|开始||
<!-- more -->
## 核心特效
```
MVC
模块化和依赖注入
双向数据绑定
指令
```

## Controller 使用过程中的注意点
```
不要试图去复用Controller,一个控制器一般只负责一小块试图
不要在Controller中操作DOM,这不是控制器的职责     指令
不要在Controller里面做数据格式化,ng有很好用的表单控件
不要在COntroller里面做数据过滤操作,ng有$filter服务
一般来说,Controller是不会互相调用的,控制器之间的交互会通过事件进行
```

> 数据模式(Model)

> 视图(View)

> 控制器(Controller)

## AngularJS的MVC是借助$scope实现的，就是说全部借助于作用域实现的

## 神器的$scope
```javascript
$scope 是一个POJO(Plain Old javascript)  普通的js对象pojo
$scope 提供了一些工具方法$watch()实时监测属性变化/$apply() 一般来说不会手动调用，它会帮我们在内部监控这些属性的实时变化
$scope 是表达式的执行环境(或者叫作用域)
$scope 是一个树形结构,与DOM标签平行，子层标签继承父标签上面的一些属性，方法
每一个Angular应用只有一个根$scope对象(一般位于ng-app上)
$scope 可以传播事件，类似DOM事件，可以向上也可以向下
$scope 不仅是MVC的基础，也是后面实现双向数据绑定的基础
可以用angular.element($0).scope()进行调试


angular启动的时候一定首先创建$rootScope,子层的$scope都是不断的往$rootScope上面添加的
当然子层标签也是可以创建一些独立的$scope

$scope的生命周期
    Creation 创建
    Watcher registration 监控
    Model mutation 检测模型的变化
    Mutation observation 观察模型到底有没有(脏啊) 
    Scope destruction 不用了可以销毁
```

## 用到的指令记录
```javascript
ng-repeat 迭代数组
$emit() 向上去传播 向根          树型结构
$broadcast() 向下去传播 向叶子   树型结构
ng-band 避免出现{{}} 首页用ng-band 子页面因为已经被angular管理了，所以可以用{{}}
ng-model 没有具体讲解，不过看起来和上面挺像   <input>, <select>, <textarea>, 元素支持该指令。
ng-class 接受表达式 ng-class='{moss:Nime,asd:Asd}' 如果Nime为true,则赋值moss
ng-show 显示
ng-hide
```

## 路由、模块、依赖注入
```javascript
使用 ngRoute 进行视图直接的路由
$routeProvider  有两个方法
    when        /demo
    otherwise   默认跳转

```

## github 搜索  Angular-ui项目

## 指令
```javascript
myModule.directive("hello",function(){
    return {
        restrict: 'AEMC',  E 元素 C class A 自定义属性 M 注释<!-- directive:hello --> 注意两边有空格   
        E A 常用
        template: '<div>Hi everyone!</div>',
        replace: true

        templateUrl: 'hello.html'

        transclude: true 可以嵌套
        template: "<div>Hi everyone!<div ng-transclude></div> </div> "
 
        compile: function(){  自定义compile

        }

        link: function(scope,element,attr){ 操作DOM 绑定事件，作用域
            
        }

        controller: function($scope) {   应该是暴露一组函数，别人可以调用。给外部调用
            $scope.abilities = [];
            this.addStrength= function() {
                $scope.abilities.push('strength')
            }
        }
        require: '^superman',  依赖于superman指令  这样就可以调用到里面的controller
        link: function(scope,element,attrs,supermanCtrl) {   supermanCtrl 就可以调用到依赖superman指令暴露出的controller函数

        }
    }
    })
```