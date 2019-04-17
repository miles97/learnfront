## Vue项目实战问题小结

首先即是关于组件的复用，因为使用情况并不一样，需要了解最终渲染的页面样式以及其他的父页面逻辑。

然后就是研究整体开发逻辑，开发组件或者直接开发页面

merge或者 push之前研究参数以及名称的细节问题

### 实际问题

项目中需要使用echarts
然后按照官方文档觉得webpack板块的echart过分麻烦，目前技术选址vue-echarts
同样也是官方维护的echarts,不过同其他npm项目一直，也需要依赖引入

重构项目中很多时候为了方便，使用的样式以及页面代码都是老的东西，只不过将一些数据渲染的方式改变了一下，实际上还是没有达到相应的要求。
不过为了时间考虑，项目排期也不允许用一些新的样式改造

所以重构的考量好像要比新项目更复杂

目前的问题卡在日期选择组件的应用   echarts数据的导入 以及 scroll-x 表单和样式的调整问题，还有切换页面，直接切换不同的页面还是重新加载不同的图表和数据

### Vue-echarts数据填充问题

在接口成功之后处理数据  
当下的问题是如何动态刷新echats的数据，使得重新加载并且拿到数据

this.chart.setOption(this.orgOptions)

然后对于不同的图表数据，进行不同层次的独立封装，每个函数回调相应配置参数
getBarCharts(){
    return {
        legeng:{},
        series:{},
        …………
    }
}

然后在trigger方法中
this.orgOptions=this.getBarCharts();
this.chart.setOption(this.orgOptions);

初步完成charts的init


## 扫盲篇

因为之前用过小程序开发，不过已经三个月之前，此内并没有深刻的接触小程序的用法，导致了很多内容都忘记了。特别是一些关键的用法，vue中也是相似的使用。

当我们需要选中渲染表单而且有一个√显示的时候

```
<div class="mapList" v-if="isShow">
    <div class="list-line" v-for="(item,index) in mapList" :key="index" @click="changePage(index)" >
        <p :class="isActive[index] ? 'active' : '' ">{{item}}<i class="icon" v-if="isActive[index]===true"></i></p>
    </div>
    <div class="mask"></div>
</div>

changePage(index){
    this.isActive = [false,false,false,false,false,false]
    this.isActive[index] = true;
    console.log(event.target)
}

```

### 开发问题一览

1.关于echarts图标的页面内切换

基本想法是直接请求数据，然后重新setdata，再进行页面的渲染


2.研究项目重构之前的代码以及内容取舍

因为功能即是echarts和滑动的展示，所以问题并不大

3.关于未知接口而定义页面的问题

造成渲染遗留问题，而且对于数据格式的问题也有很大

4.
点击指标说明图标，可查看各指标的含义
门店筛选：选择查看具体门店数据
点击“指标名称”，进行隐藏和显示
统计时间筛选：选择查看数据的时间范围，默认当月

## v-for 渲染

渲染value  item in list{{item}}

渲染带index的value item,index in list   {{index}}  {{value}}

渲染key  (item,key) in list {{key}} {{value}}

如果需要条件从json里面筛选数据，即可直接从v-if=""表达式内完成筛选，不需要js处理数据

### 处理较长的字符串
```
overflow-x: hidden;
text-overflow: ellipsis;
white-space: nowrap;
 ```

## 关于h5的基础问题

太多了 特别是关于渲染当中的div ul li使用

i span 等常用小标签的使用

还有定位posion,以及float的滥用问题

### router.js  

路由#产生的原因：http://127.0.0.1:8088/#/addNewBusiness

router:history模式以及hash

关于run server的问题

webpack.config配置项中直接 0.0.0.0默认。本机ip地址加上端口即可在本地局域网访问，

如果是vue-cli，直接应该在package.json中默认配置

```bash
npm run serve
"serve": "vue-cli-service serve",
```

## localStorage

表达输入的友好型问题即是
在一些需要选择的选项中用set存储数据，在下一次接口请求成功的时候将localStorage取出，填充。

### 联动picker选择

mint-ui // better-scroll

### 关于深入理解组件内容

### Vue-echarts踩坑手册

首先关于渐变色

渐变色需要引入一个东西
new echarts.graphic.LinearGradient()
如果直接使用是会报错误的

[areaStyle参数说明](https://echarts.baidu.com/option.html#series-line.areaStyle.color)

在vue中的使用，需要一系列之前的内容，详细的包括

main.js声明

import ECharts from 'vue-echarts/components/ECharts'
Vue.component('chart', ECharts)

页面内引入

import ECharts from 'vue-echarts/components/ECharts.vue'
import 'echarts/lib/component/tooltip'
import 'echarts/lib/component/legend'
import 'echarts/lib/chart/line'
import echarts from 'echarts'

方法中声明

我写在了

mounted(){
	this.echarts = echarts

}

然后可以使用

还有一点即是areaStyle.color是填充性质，如果没有放在areaStyle里面。默认的渐变选项即是线条的渐变色

差别如下

面积颜色填充
areaStyle: {
    type: 'default',
    color: new echarts.graphic.LinearGradient(
        0, 0, 0, 1,
        [
            {offset: 0, color: 'rgba(80,141,255,0.39)'},
            {offset: 0.5, color: '#fff'},
            {offset: 1, color: '#fff'}
        ]
    )
},
线条渐变色填充
areaStyle: {
    type: 'default',
},
color: new echarts.graphic.LinearGradient(
    0, 0, 0, 1,
    [
        {offset: 0, color: 'rgba(80,141,255,0.39)'},
        {offset: 1, color: 'rgba(38,197,254,0.00)'}
    ]
)
#### 完整的vue-echarts配置代码
```
this.orgOptions = {
                tooltip: {
                    trigger: 'axis'
                },
                toolbox: {
                    show: true,
                    feature: {
                        mark: {show: true},
                        dataView: {show: true, readOnly: false},
                        magicType: {show: true, type: ['line', 'bar', 'stack', 'tiled']},
                        restore: {show: true},
                        saveAsImage: {show: true}
                    }
                },
                xAxis: {
                    type: 'category',
                    boundaryGap: false,
                    data: ['02/07', '02/08', '02/10', '02/12', '02/15', '02/17', '02/20', '02/22', '02/25', '02/28', '03/01', '03/03', '03/05', '03/07', '03/10'],
                    axisLabel: {interval: 1},   //控制隐藏x轴坐标
                    axisLine: {lineStyle: {color: '#ccc'}}
                },
                yAxis: {type: 'value', axisLine: {lineStyle: {color: '#ccc'}}, axisTick: {show: false}},
                series: [
                    {
                        symbol: "none",
                        type: 'line',
                        data: [5, 3, 6, 8, 9, 3, 2, 3, 4, 6, 8, 9, 3, 4, 5],
                        smooth: true,
                        color: 'rgb(80,141,255)',
                        itemStyle: {
                            normal: {
                                areaStyle: {
                                    type: 'default',
                                    color: new echarts.graphic.LinearGradient(
                                        0, 0, 0, 1,
                                        [
                                            {offset: 0, color: 'rgba(80,141,255,0.39)'},
                                            {offset: 0.5, color: '#fff'},
                                            {offset: 1, color: '#fff'}
                                        ]
                                    )
                                },

                            }
                        },
                        name: 'demo1'
                    },
                    {
                        symbol: "none",
                        type: 'line',
                        data: [4, 5, 3, 2, 4, 5, 4, 3, 5, 3, 2, 4, 5, 4, 3],
                        smooth: true,
                        itemStyle: {
                            normal: {
                                areaStyle: {
                                    type: 'default',
                                },
                                color: new echarts.graphic.LinearGradient(
                                    0, 0, 0, 1,
                                    [
                                        {offset: 0, color: 'rgba(80,141,255,0.39)'},
                                        {offset: 1, color: 'rgba(38,197,254,0.00)'}
                                    ]
                                )
                            }
                        },
                        name: 'demo2'
                    },
                    {
                        symbol: "none",
                        type: 'line',
                        data: [4, 3, 4, 3, 3, 8, 7, 6, 3, 4, 3, 3, 8, 7, 6],
                        smooth: true,
                        itemStyle: {
                            normal: {
                                areaStyle: {
                                    type: 'default', color: new echarts.graphic.LinearGradient(
                                        0, 0, 0, 1,
                                        [
                                            {offset: 0, color: 'rgba(80,141,255,0.39)'},
                                            {offset: 1, color: 'rgba(38,197,254,0.00)'}
                                        ]
                                    )
                                }
                            },
                        },
                        name: 'demo3'
                    },
                    {
                        symbol: "none",
                        type: 'line',
                        color: 'yellow',
                        data: [4, 2, 3, 4, 8, 3, 5, 8, 4, 5, 4, 8, 3, 5, 8],
                        smooth: true,
                        itemStyle: {
                            normal: {
                                areaStyle: {
                                    type: 'default', color: new echarts.graphic.LinearGradient(
                                        0, 0, 0, 1,
                                        [
                                            {offset: 0, color: '#fff'},
                                            {offset: 1, color: '#fff'}
                                        ]
                                    )
                                }
                            },
                        },
                        name: 'demo4'
                    },
                ]
            };
```

Echarts配置

详细配置
https://echarts.baidu.com/option.html#series-line.itemStyle.borderType

example

https://echarts.baidu.com/echarts2/doc/example.html

https://echarts.baidu.com/examples/

CSDN教程
https://blog.csdn.net/sleepwalker_1992/article/details/82709793


### Echarts全屏模式

点击缩放按钮，全屏显示

横纵向布局的设置，一般在『组件』或者『系列』的 orient 或者 layout 配置项上，设置为 'horizontal' 或者 'vertical'

1. echarts横向渲染，不改变div


2. div tranform:ratote(90deg)

直接旋转90度，通过position等定位重新拉回来页面

3. 直接通过x轴和y轴的相关设置进行

参考
https://blog.csdn.net/qq_40594137/article/details/80896631

。。。。toolTips并不能旋转

4. 通过嵌套iframe，然后引入进行旋转



### Echarts数据展示

因为echarts4z之后引入的dataset模式，使用回调函数或者字符串进行输入的带入


### 自定义组件时候的基本思想

以使用Mint-ui的picker为例  当我们需要两种数据的时候

仍然还是使用solts和value结合的方式展示数据，此外还是需要对于数组的内容进行处理，
才能进行两种数据的展示。


```
let province=[];
for(let i=0;i<data.data.length;i++){
    province.push(data.data[i].province);
    let city=[]
    for(let j=0;j<data.data[i].cityList.length;j++){
    city.push(data.data[i].cityList[j].city)
}
 that.cityList[data.data[i].province]=city;
}
that.slots[0].values=province;
that.slots[1].values=that.cityList[data.data[0].province];
```

当然现在对于组件的理解还不是很深刻，所以基本上的还是处于理解状态中

如果页面样式不一致，仍然上，实际上需要复用的组件只是popup的部分，
所以需要分辨实际用处部分，然后再通过这些内容进行交互(实际上Api已经说明了slots的使用方法)

### 关于组件化的内容
写一个组件的具体系统化思维

getValue   getDefalut 等等重复的逻辑

mounted create等生命周期的含义

关于mounted和created的差别，mounted是在模板渲染完成之后的周期，而created是在模板渲染之前的
所以涉及到dom操作的防范写在mounted内，其他基本写在created内，类似于onload

在自己倒腾了好几周的组件中，还是觉得自己的时间以及认知水平不足以支撑自己完成一个组件，至今也没有做出来日期组件，省市两级联动的组件，还有echarts的动态加载也没完成。

emmmm
在这几天闲里偷忙的时间，做一些codereview，提升自己代码水平，观察同事的代码

初步发现自己的布局方面还是存在很大的问题，特别是对于一些position,display和float。

还有flex的理解真的不是很深刻

### Vue组件技术栈

vue-photo-preview  //图片预览加载 mounted() {this.previewRefresh(); },

还有个问题即是  我怎么就没用到emit,等等的功能vue store



### Vue特殊含义小结

```
this.$refs[item.ref].$el.classList.add('clue-fail-dd');


```