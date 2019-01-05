# vuex

## 描述

本文档对于有一定`vue`项目环境搭建和配置的相关经验的人会更轻松的理解

## 什么是 vuex

> vuex 是 Vue 配套的公共管理数据工具，他可以把一些共享的数据，保存到 vuex 中，方便整个程序中的任何组件直接获取或者修改公共数据

## 为什么要用vuex

下面原始的传值和vuex的区别与优缺点

### 兄弟组件之间传值

定义中间实例`vm`进行传值，逻辑过于复杂

### 父子组件之间传值

子向父传值，通过事件调用机制，父给子传值通过属性绑定，但如果嵌套过深的传值，难免会产生复杂的逻辑

### 引入vuex

vuex相当于是项目共享数据仓库，全局共享数据存储区域，方便各个组件直接拿来使用，这样就少了很多复杂的逻辑

![1546667489888](C:\Users\wanggongtou\Desktop\myHexoBlog\source\_posts\assets\1546667489888.png)

## vuex安装和使用

1. 安装

```shell
npm i vuex -S
```

1. 导入包到文件

```javascript
import Vuex from 'vuex'
```

1. 注册

```javascript
Vue.use(Vuex)
```

1. 实例化，得到一个数据仓储对象

```javascript
var store = new Vuex.store({
    state: {
        // 可以把 state 对比 vue 组件中的 data ，专门用来存储数据
        count: 0
    },
    mutations: {
        
    },
    getters: {
        
    }
})
```

1. 将 vuex 创建的实例对象挂载到 vm 实例上

```javascript
const vm = new Vue({
  ...
  // store: store 
  store
})
```

1. 在需要共享数据的组件中想要去访问 vuex 实例中的数据，需要通过`this.$store.count`来访问

### 例子

#### 项目演示地址

[]()可以先去这里下载项目下来，跟着例子操作

创建基本项目结构和搭建vue环境和配置，不多赘述了

![1546668981777](C:\Users\wanggongtou\Desktop\vuex-study\assets/1546668981777.png)

