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

首先就是@，对于绑定的操作之外，v-on更多的支持了键盘修饰符的惭怍，如key-up等操作。以及一些防止事假冒泡的操作内容。@click.stop.甚至可以绑定一个动态事件

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


```bash
所有的prop都使得其父子prop之间形成了一个单向下行绑定：父级prop的更新会向下流动到子组件中，但是反过来则不行。这样会防止从子组件意外改变父级组件的状态，从而导致你的应用的数据流向难以理解。


父组件在调用子组件时用v-on把事件传给子组件，子组件用this.$emit调用父组件传过来的事件。
```

### 绑定class  {active : isActive},以及绑定props子父组件传参

·在对象当中，CSS 的属性名要用驼峰式表达：fontSize 解析成 font-size

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