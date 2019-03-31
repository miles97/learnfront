## Vue原生项目研究小结

### 之前自己的逻辑都是通过模仿页面实现，然后请求接口完成。

### 在Vue原生项目中。组件的应用更广泛而且页面结构更加复杂，路由，以及自定义的内容逻辑。
页面耦合逻辑

然后之前有做过微信小程序的页面分析，关于vue的页面布局如下
```
├── build                                       // webpack配置文件，
├── config                                      // 项目打包路径。阅读配置手册进行配置
├── elm                                         // 上线项目文件，放在服务器即可正常访问
├── src                                         // 源码目录
│   ├── components                              // 组件||通用组件，购物车，列表，评价等。
│   ├── config                                  // 基本配置||rem转换，baseUrl,处理参数
│   ├── page									// pages页面|页面内的逻辑
│   ├── router									// 路由配置,说明所有页面的路径
│   ├── service                                 // 数据交互统一调配||用接口拿数据
│   ├── store                                   // vuex的状态管理????虽然暂时用不到
│  	├── style  									// 样式文件目录
│   ├── App.vue                                 // 页面入口文件||组件在各个page的正确引用	
│   └── main.js                                 // 程序入口文件，加载各种公共组件
├── favicon.ico                                 // 图标
├── index.html                                  // 入口html文件
```
## 浅显研究的话就是vue更多的是一些状态管理以及跨页面的通信，通过多重复用的组件实现代码，页面。


# Vue 内容详细比较

## 学习路径
```
|_____v-on//v-bind[√]
|
|_____组件基础，事件处理[√]
|
|_____上手向(教程)||计算属性，侦听属性对于wxmp的比较
|
|_____生产代码中校对的问题以及组件复用
|
|_____特殊处理，常见实例方法属性||$emit，$mount
|
|_____API向学习||全局API，全局配置，内置组件，指令||VUEX，vue-router,vue-cli
|
|_____cookbook以及风格指南


```
## 关于v-on ||@

首先就是@，对于绑定的操作之外，v-on更多的支持了键盘修饰符的操作，如key-up等操作。以及一些防止事假冒泡的操作内容。@click.stop.甚至可以绑定一个动态事件

```
<!-- 动态事件 (2.6.0+) -->
<button v-on:[event]="doThis"></button>
<!-- 内联语句 -->
<button v-on:click="doThat('hello', $event)"></button>
<!-- 点击回调只会触发一次 -->
<button v-on:click.once="doThis"></button>
<!-- 对象语法 (2.4.0+) -->
<button v-on="{ mousedown: doThis, mouseup: doThat }"></button>

```
### 在子组件上监听自定义事件 (当子组件触发“my-event”时将调用事件处理器)：

```
<my-component @my-event="handleThis"></my-component>

<!-- 内联语句 -->
<my-component @my-event="handleThis(123, $event)"></my-component>

<!-- 组件中的原生事件 -->
<my-component @click.native="onClick"></my-component>


```

### [事件处理器](https://cn.vuejs.org/v2/guide/events.html)

有时也需要在内联语句处理器中访问原始的 DOM 事件。可以用特殊变量 $event 把它传入方法：
```
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>

methods: {
  warn: function (message, event) {
    // 现在我们可以访问原生事件对象
    if (event) event.preventDefault()
    alert(message)
  }
}
```

### [组件基础](https://cn.vuejs.org/v2/guide/components.html)

### [通过 Prop 向子组件传递数据](https://cn.vuejs.org/v2/guide/components.html#%E9%80%9A%E8%BF%87-Prop-%E5%90%91%E5%AD%90%E7%BB%84%E4%BB%B6%E4%BC%A0%E9%80%92%E6%95%B0%E6%8D%AE)

尚不是很理解

## v-bind || :

动态绑定多个特性以及组件prop到表达式
```
<!-- prop 绑定。“prop”必须在 my-component 中声明。-->
<my-component :prop="someThing"></my-component>
<!-- 通过 $props 将父组件的 props 一起传给子组件 -->
<child-component v-bind="$props"></child-component>
```

## 为什么使用Props传递样式属性以及其他回调函数?

在页面和组件中，组件的复用相当频繁，所以将一些需要的组件属性，定义成prop。
然后在子组件内定义数据类型以及默认值，在父组件内获取以及传递值。

所以所有的prop都是单项数据流的。


```
所有的prop都使得其父子prop之间形成了一个单向下行绑定：父级prop的更新会向下流动到子组件
中，但是反过来则不行。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。


父组件在调用子组件时用v-on把事件传给子组件，子组件用this.$emit调用父组件传过来的事件。
```

### 绑定class  {active : isActive},以及绑定props子父组件传参

·在对象当中，CSS 的属性名要用驼峰式表达：fontSize解析成font-size

```
<button class="licenses-button" :disabled="disabled" :style="stylecss" @click="callLicenses">{{keyText}}</button>

props: {
    stylecss: { //输入框样式
    type: Object
},
}

//多个动态绑定的样式
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>

data: {
  isActive: true,
  hasError: false
}
//结果渲染为
<div class="static active"></div>

```
## 路由规则

### 定义 component
```javascript
const reportList = r => require.ensure([], () => r(require('@/pages/reportList/')), 'home')



```
### 定义路由规则
```javascript
routes: [
  {
    path:'/',
    name:'reportList',
    redirect:'/reportList',
  }]
```
### 使用
```
   <div id="app">
        <h1>Hello VueRouter</h1>
        <p>
            <!-- 使用 router-link 组件来导航. -->
            <!-- 通过传入 `to` 属性指定链接. -->
            <!-- <router-link> 默认会被渲染成一个 `<a>` 标签 -->
            <!-- 属性 `to` 对应生成  `<a>` 标签的 `href` 属性-->
            <router-link to="/foo">Foo</router-link>
            <router-link to="/bar">Bar</router-link>
        </p>
        <!--路由匹配的组件在此处渲染-->
        <router-view></router-view>
    </div>
```

## 使用组件化方法的mint-Ul

## 页面标签

### 按需加载
```java
import {Header,Toast,Field,Button,Indicator} from 'mint-ui';
```
### 页面公用方法在main.js中引入
goback&&tourl等

其他组件定义可以在components内自定义名称

## 代码逻辑（多页面管理）vuex

实际上的使用还不是很清楚

大概就是放置了五个原生方法，然后组件通过mapState函数进行使用

#### mapState辅助方法
```javascript
import { mapState } from 'vuex'

export default {
    computed: mapState ({
        count: state => state.count,
        countAlias: 'count',    // 别名 `count` 等价于 state => state.count
    })
}
```
其他同时也可以放一下公用方法

### 关于State、Getter、Muataion、Action、Module的作用问题

#### Getter
```
如果我们需要对state对象进行做处理计算，如下：

computed: {
    doneTodosCount () {
        return this.$store.state.todos.filter(todo => todo.done).length
    }
}

如果多个组件都要进行这样的处理，那么就要在多个组件中复制该函数。这样是很没有效率的事情，当这个处理过程更改了，还有在多个组件中进行同样的更改，这就更加不易于维护。

Vuex中getters对象，可以方便我们在store中做集中的处理。Getters接受state作为第一个参数：

const store = new Vuex.Store({
  state: {
    todos: [
      { id: 1, text: '...', done: true },
      { id: 2, text: '...', done: false }
    ]
  },
  getters: {
    doneTodos: state => {
      return state.todos.filter(todo => todo.done)
    }
  }
})

在Vue中通过store.getters对象调用。

computed: {
  doneTodos () {
    return this.$store.getters.doneTodos
  }
}

Getter也可以接受其他getters作为第二个参数：

getters: {
  doneTodos: state => {
      return state.todos.filter(todo => todo.done)
  },
  doneTodosCount: (state, getters) => {
    return getters.doneTodos.length
  }
}
```

就是在getter里面是一些计算属性的公用方法，通过在vuex里面的状态注册之后可以进行所有页面的访问引用，完成组件化的思想以及实现。
与一般的公用方法不同的是，getter的效率更高而且有一些特定方式访问，并且可以多个参数进行调用。

#### [mapGetters辅助函数](https://juejin.im/entry/58cb4c36b123db00532076a2)

略过

#### Mutations

vuex中唯一用来更改state中的状态的方法

每一个mutation都有一个事件类型type和一个回调函数handler
```

const store = new Vuex.Store({
  state: {
    count: 1
  },
  mutations: {
    increment (state) {
      // 变更状态
      state.count++
    }
  }
})
```
```
调用mutation，需要通过store.commit方法调用mutation type：
store.commit('increment')
```

##### Payload 提交载荷
也可以向store.commit传入第二参数，也就是mutation的payload:
```
mutaion: {
    increment (state, n) {
        state.count += n;
    }
}

store.commit('increment', 10);
```

```
不例外，mutations也有映射函数mapMutations，帮助我们简化代码，使用mapMutations辅助函数将组件中的methods映射为store.commit调用。

import { mapMutations } from 'vuex'

export default {
  // ...
  methods: {
    ...mapMutations([
      'increment' // 映射 this.increment() 为 this.$store.commit('increment')
    ]),
    ...mapMutations({
      add: 'increment' // 映射 this.add() 为 this.$store.commit('increment')
    })
  }
}
注 Mutations必须是同步函数。

如果我们需要异步操作，Mutations就不能满足我们需求了，这时候我们就需要Actions了

```

### Action
分发 Action
Action 通过 store.dispatch 方法触发：
```
store.dispatch('increment')

 mutation 必须同步执行,而action 内部执行异步操作：

actions: {
  incrementAsync ({ commit }) {
    setTimeout(() => {
      commit('increment')
    }, 1000)
  }
}
```
### VueX独立模块

如果想要独立store中的其他方法，也可以通过创建index.js引入相应文件进行
```
import Vue from 'vue'
import Vuex from 'vuex'
import mutations from './mutations'
import actions from './action'
import getters from './getters'
```
最后将接口暴露出来，进行调用。state还是不变，在index.js中进行声明。

### 其他内容

[前端性能优化](https://segmentfault.com/a/1190000018263418)


[立即执行函数与闭包](https://my.oschina.net/u/2331760/blog/468672?p={{currentPage+1}})


[理解闭包](https://www.cnblogs.com/wangfupeng1988/p/3994065.html)


[前端面试题 VUE](https://segmentfault.com/a/1190000018634744)


[前端interview精华](https://juejin.im/post/5c64d15d6fb9a049d37f9c20)


[小程序实现原理解析](https://juejin.im/post/5c970d16f265da612d634475)



## 项目解析[RSS-READER](https://github.com/mrgodhani/rss-reader/releases)

学习目的：vue的代码规范以及国外的技术栈差别

生命周期函数写在main.vue里面

组件方法不写在组件内，this.$store代理



## 如何从零开始构建一个健康的vue项目以及大型的单页面项目框架

### 针对简单的小型单页面项目
(对比小程序)

首先可以不需要很多公用方法的封装以及其他资源的引用，特别是vueX等状态管理器，直接在组件内运用相关的计算方法即可。不需要考虑组件复用以及其他逻辑。路由等也不需要考虑的很复杂。

小程序，很多内容可以直接不用考虑，只需要封装一些简单的请求方法或者是公用的数据处理方法。
像是对于对象，数组的公用处理方法，ajax请求，API接口。
其他只需要关注页面本身的逻辑即可，
由于vue本身的资源丰富，所以可以使用的组将更多，复杂度较小程序也更高。

```
this.$store.commit('stateFn',obj); //状态管理  


// 将需要更新的值传到vuex,然后通过mutation方法(vuex中唯一用来更改state中的状态的方法)更改目标值

```

复杂的问题在于，很多请求的实现都是用了许许多多不同的组件实现的。而这些组件之中又有着不同的依赖，部分也都是相互依赖的modules。
像是axios，h5_native_proxy(自定义),proxy(?)等

使用辅助函数mapstate的作用即是
当一个组件需要获取多个状态时候，将这些状态都声明为计算属性会有些重复和冗余。

通过状态管理辅助函数完成对于单一状态树下的模块化管理

### 针对大型的多页面项目(elm)
<!-- 目前并不针对css3以及scss有涉及 -->

因为页面数量众多，相应的，引入的组件也更多，对于状态的操作也更频繁。页面之间的跳转逻辑颗牙哟独立出来，同时，考虑代码的维护性
很多功能并不需要写在store中，直接在页面内使用即可。

而且一般的页面逻辑并不复杂，都是一些简单方法的调用，getData，传参，数据处理，组件页面因为涉及到复用而更加繁琐。

大型项目的vuex store写法即是，将一些types单独暴露在其他的js中，然后在需要引入的地方
正确引入，然后将其用标准语法

#### again 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。

```
 increment (state) {
      // 变更状态
      state.count++
    }
以上的标准写法，过渡成
  [INIT_BUYCART](state) {
    ……………………表达式
},

这种写法是 ES2015 风格的计算属性命名功能来使用一个常量作为函数名

这个称之为type，意义就是唤醒上面的  handler,

store.commit('increment')

为store.commit 传入额外的参数，即 mutation 的 载荷（payload）

store.commit('increment', {
  amount: 10
})



```