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

[git@github.com:ForeManWang/vuex-study.git](git@github.com:ForeManWang/vuex-study.git)

可以先去这里下载项目下来，跟着例子操作

创建基本项目结构和搭建vue环境和配置，不多赘述了

![1546668981777](C:\Users\wanggongtou\Desktop\myHexoBlog\source\_posts\assets\1546668981777.png)

下载下来项目之后，在根目录自行安装所有依赖插件

```javascript

```

##### state

这里就是相当于第六步，去`components/amount.vue`

去访问 vuex 实例中的数据

![1546670935396](C:\Users\wanggongtou\Desktop\myHexoBlog\source\_posts\vue-vuex\1546670935396.png)

这时候打开后台，打开页面刷新，发现就已经能够访问 vuex 中的数据了

##### moutations 操作数据

**加法需求**

去`components/counter.vue `的`methods`中写了一个`add`方法

```html
    <input type="button" value="增加" @click="add">
```



```javascript
    add() {
      // 千万不要这么用，不符合 vuex 的设计理念
      // 这种操作数据的方法相当于是自己的组件操作数据，假如数据紊乱，会不知道是谁操作的数据导致了数据紊乱，所以不利于后期维护
      // 所以将需求告诉一个 <库管员> 让 <库管员> 去操作
      // 这里就是要 vuex 仓库中的 mutations 提供的方法去操作对应的数据，这样假如维护中出现数据紊乱的情况，可以快速定位错误的原因，利于后期维护
       this.$store.state.count++;  
    },
```

**注意**： 如果组件想要调用 mutations 中的方法，只能使用 this.$store.commit('方法名')

所以在`components/counter.vue `内部就这样调用

```javascript
    add() {
      // 千万不要这么用，不符合 vuex 的设计理念
      // this.$store.state.count++;
      this.$store.commit("increment");
    },
```

**减法需求**

去`components/counter.vue `的`methods`中写了一个`remove`方法

```html
<input type="button" value="减少" @click="remove">
```

在`main.js`的`moutations`中定义

```javascript
    subtract(state, obj) {
      state.count -= (obj.c + obj.d)
    }
```

**注意**： mutations 的 函数参数列表中，最多支持两个参数，其中，参数1： 是 state 状态； 参数2： 通过 commit 提交过来的参数；

去`components/counter.vue `中调用，需要传参

```javascript

```

这样减法就实现了，实现一次性减`obj.c + obj.d`

##### getters 包装数据

> 这里的 getters， 只负责 对外提供数据，不负责 修改数据，如果想要修改 state 中的数据，请 去找 mutations

`main.js`

```javascript
  getters: {
    optCount: function (state) {
      return '当前最新的count值是: ' + state.count
    }
  }
```

`counter.vue`

```javascript
  computed:{
    fullname: {
      get(){},
      set(){}
    }
  }
```

经过回顾对比，发现 getters 中的方法， 和组件中的过滤器比较类似，因为过滤器和 getters 都没有修改原数据， 都是把原数据做了一层包装，提供给了 调用者；
其次， getters 也和 computed 比较像， 只要 state 中的数据发生变化了，那么，如果 getters 正好也引用了这个数据，那么 就会立即触发 getters 的重新求值

#### 总结

1. state中的数据，不能直接修改，如果想要修改，必须通过` mutations`
2. 如果组件想要直接 从 state 上获取数据： 需要 `this.$store.state.***`
3. 如果 组件，想要修改数据，必须使用 `mutations` 提供的方法，需要通过 `this.$store.commit('方法的名称'， 唯一的一个参数)`
4. 如果 store 中 state 上的数据， 在对外提供的时候，需要`做一层包装`，那么 ，推荐使用` getters`, 如果需要使用 `getters` ,则用` this.$store.getters.***`