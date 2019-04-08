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

目前的问题卡在日期选择组件的应用   echarts数据的导入 以及 scroll-x 表单和样式的调整问题，还有切换页面，直接切换不同的页面还是重新加载不同的图表和 数据

对于使用过一次的技术，自己还是印象不深刻，造成了很多时间的浪费，在之后KPI考核的标准下，还是应该自己捡起来猥琐发育才行

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