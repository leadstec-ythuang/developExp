[TOC]

# ECharts

## 配置

### 下载
+ 在ECharts 的 [GitHub](https://github.com/apache/echarts/releases) 获取。
+ npm获取`npm install echarts --save`

### 引入页面
1. `<script src="echarts.min.js"></script>`
2. 配置具备宽高的容器`<div id="main" style="width: 600px;height:400px;"></div>`
3. 通过`echarts.init`方法初始化
`var myChart = echarts.init(document.getElementById('main'));`
4. 配置`option`数据
5. 通过 `setOption` 方法生成图表`myChart.setOption(option);`

## 语法概念
### series(系列)
表示一组数值以及他们映射成的图,包含的要素至少有：一组数值、图表类型（series.type）、以及其他的关于这些数据如何映射成图的参数

### component(组件)
#### xAxis（直角坐标系 X 轴）
#### yAxis（直角坐标系 Y 轴）
#### grid（直角坐标系底板）
#### tooltip（提示框组件）
#### series(系列)
#### toolbox（工具栏组件）

## 样式

### 阴影
在series的`itemStyle`设置
```js
itemStyle: {
    // 阴影的大小
    shadowBlur: 200,
    // 阴影水平方向上的偏移
    shadowOffsetX: 0,
    // 阴影垂直方向上的偏移
    shadowOffsetY: 0,
    // 阴影颜色
    shadowColor: 'rgba(0, 0, 0, 0.5)',
    // 鼠标 hover 时候的高亮样式
    emphasis: {
        shadowBlur: 200,
        shadowColor: 'rgba(0, 0, 0, 0.5)'
    }
}
```

### 背景色
全局设置
`setOption({})`
#### 自定义主题
在[主题编辑器](https://echarts.apache.org/zh/theme-builder.html)访问编辑并下载
+ 保存为JSON文件
```js
// 假设主题名称是 "vintage"
$.getJSON('xxx/xxx/vintage.json', function (themeJSON) {
    echarts.registerTheme('vintage', JSON.parse(themeJSON))
    var chart = echarts.init(dom, 'vintage');
});
```
+ UMD格式的Js文件
```js
// HTML 引入 vintage.js 文件后（假设主题名称是 "vintage"）
var chart = echarts.init(dom, 'vintage');
// ...
```
#### 背景颜色
`backgroundColor: '#2c343c'`

#### 文本样式
`textStyle: {color: 'rgba(255, 255, 255, 0.3)'}`

###### 为每个系列分别设置文本样式
在`label.textStyle`中设置
```js
label: {
    textStyle: {
        color: 'rgba(255, 255, 255, 0.3)'
    }
}
```

饼图需要将标签视觉引导线颜色设为浅色
```js
labelLine: {
    lineStyle: {
        color: 'rgba(255, 255, 255, 0.3)'
    }
}
```



