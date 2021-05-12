[TOC]

# 百度地图

## 使用
### 申请账号和秘钥
[申请秘钥-ak](http://lbsyun.baidu.com/apiconsole/key?application=key)

### 3D地图渲染
支持3D地图,无极缩放,调整地图视野倾斜角,旋转角度和地球模式。支持核心属性的配置，鼠标交互事件等

#### Map接口

### 控件
控件是负责与地图交互的UI元素
#### 抽象基类(Control)
所有控件均继承此类的方法、属性。通过此类您可实现自定义控件
#### 比例尺控件(ScaleControl)
根据地图缩放级别自动调整比例尺长度和单位,默认位于地图左下方，显示地图的比例关系

#### 缩放控件(ZoomControl)
默认位于地图右下方，控制地图级别的缩放

#### 定位(LocationControl)
默认位于地图左下方，用于获取定位

#### 城市选择列表(CityListControl)
默认位于地图左上方，用于进行城市选择定位

#### 版权(CopyrightControl)
默认位于地图左下方，用于展示版权信息

##### 声明控件并添加到地图中
```js
var scaleCtrl = new BMapGL.ScaleControl();  // 添加比例尺控件
map.addControl(scaleCtrl);
var zoomCtrl = new BMapGL.ZoomControl();  // 添加缩放控件
map.addControl(zoomCtrl);
var cityCtrl = new BMapGL.CityListControl();  // 添加城市列表控件
map.addControl(cityCtrl);
```
##### 控制控件位置
初始化控件时提供参数

###### 控件的停靠位置-anchor值
+ BMAP_ANCHOR_TOP_LEFT 表示控件定位于地图的左上角
+ BMAP_ANCHOR_TOP_RIGHT	表示控件定位于地图的右上角
+ BMAP_ANCHOR_BOTTOM_LEFT 表示控件定位于地图的左下角
+ BMAP_ANCHOR_BOTTOM_RIGHT 表示控件定位于地图的右下角

###### 控件位置偏移-offset
通过偏移量来指示控件距离地图边界有多少像素
```js
var opts = {
    offset: new BMapGL.Size(150, 5)
}
map.addControl(new BMapGL.ScaleControl(opts));
```
### 覆盖物
可以使用`map.addOverlay`方法向地图添加覆盖物，使用`map.removeOverlay`方法移除覆盖物
#### 抽象基类(Overlay)
所有的覆盖物均继承此类的方法

#### 点(Marker)
表示地图上的点，可自定义标注的图标
可以通过Icon类指定自定义图标(标注的地理坐标点将位于标注所用图标的中心位置，可通过Icon的offset属性修改标定位置)
Marker的构造函数的参数为Point和MarkerOptions（可选）
+ 默认的标注样式添加标注
```js
var point = new BMapGL.Point(116.404, 39.915);   
var marker = new BMapGL.Marker(point);        // 创建标注   
map.addOverlay(marker);                     // 将标注添加到地图中
```
+ 通过Icon类实现自定义标注的图标
可通过参数MarkerOptions的icon属性/marker.setIcon()方法进行设置
```js
var myIcon = new BMapGL.Icon("markers.png", new BMapGL.Size(23, 25), {   
    // 指定定位位置。  
    // 当标注显示在地图上时，其所指向的地理位置距离图标左上   
    // 角各偏移10像素和25像素。您可以看到在本例中该位置即是  
    // 图标中央下端的尖角位置。   
    anchor: new BMapGL.Size(10, 25),   
    // 设置图片偏移。  
    // 当您需要从一幅较大的图片中截取某部分作为标注图标时，您  
    // 需要指定大图的偏移位置，此做法与css sprites技术类似。   
    imageOffset: new BMapGL.Size(0, 0 - 25)   // 设置图片偏移   
});     
    // 创建标注对象并添加到地图  
var marker = new BMapGL.Marker(point, {icon: myIcon});   
map.addOverlay(marker); 

```
+ 监听标注事件
```js
marker.addEventListener("click", function(){   
    alert("您点击了标注");   
});
```

##### 定义点
提供BMapGL.Marker3D 类进行带高度的点覆盖物绘制，支持对覆盖物点的高度、大小、形状、颜色及透明度的自定义，并可以进行纹理贴图以满足更多场景需求

> `BMapGL.Marker3D(point, height, options)` 
+ point  {Point}点坐标,通过Point创建的点对象
+ height  {numbe}点的高度
+ options  {Object}通过options自定义点的样式
    + size 点的大小,默认50
    + shape 点的形状,1为圆形,2为正方形,默认为1.(也可以使用对应的常量 BMAP_SHAPE_CIRCLE 和 BMAP_SHAPE_RECT)
    + fillColor 点的颜色，格式为 '#xxxxxx'，比如'#f00'
    + fillOpacity 点的透明度，范围0-1，默认0.8
    + icon 点的纹理贴图，格式为通过Icon创建的Icon对象
    
```js
//创建带高度的点，并添加到地图上
var marker3d = new BMapGL.Marker3D(point, 8000, {
    size: 50,
    shape: BMAP_SHAPE_CIRCLE,
    fillColor: '#454399',
    fillOpacity: 0.6
});
// 将点添加到地图上
map.addOverlay(marker3d);

//纹理图片首先需要通过Icon类创建实例，然后通过Marker3D类创建带高度的点实例时，将得到的Icon实例传给icon属性。
var icon = new BMapGL.Icon(citys[i].img, new BMapGL.Size(40, 40));
// 创建带高度的点，这里只需要size和icon
var marker = new BMapGL.Marker3D(pt, 8000, {
    size: 80,
    icon: icon
});
// 将点添加到地图上
map.addOverlay(marker);
```
#### 折线(Polyline)
折线覆盖物,包含一组点，并将这些点连接起来形成折线
[Polyline使用详情](http://lbsyun.baidu.com/cms/jsapi/reference/jsapi_webgl_1_0.html)
+ 添加折线
折线覆盖物可以分别设置描边粗细、颜色、填充颜色等属性
```js
var polyline = new BMapGL.Polyline([
		new BMapGL.Point(116.399, 39.910),
		new BMapGL.Point(116.405, 39.920),
		new BMapGL.Point(116.425, 39.900)
	], {strokeColor:"blue", strokeWeight:2, strokeOpacity:0.5});
map.addOverlay(polyline);
```
#### 多边形(Polygon)
多边形覆盖物
包含一组点。多边形将这组点按顺序首尾相连，最终围成一个封闭图形
+ 添加多边形
多边形覆盖物可以分别设置描边粗细、颜色、填充颜色等属性
```js
var polygon = new BMapGL.Polygon([
        new BMapGL.Point(116.387112,39.920977),
        new BMapGL.Point(116.385243,39.913063),
        new BMapGL.Point(116.394226,39.917988),
        new BMapGL.Point(116.401772,39.921364),
        new BMapGL.Point(116.41248,39.927893)
    ], {strokeColor:"blue", strokeWeight:2, strokeOpacity:0.5});
map.addOverlay(polygon);
```
#### 圆形(Circle)
圆形覆盖物


### 页面结构
#### 基础配置
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="initial-scale=1.0, user-scalable=no" />
    <title>Document</title>
    <style type="text/css">
        html {
            height: 100%;
        }
        body {
            height: 100%;
            margin: 0;
            padding: 0;
        }
        #container {
            height:100%
        }
    </style>
</head>
<body>
    <div id="container"></div>
    <script type="text/javascript" src="https://api.map.baidu.com/api?v=1.0&&type=webgl&ak=您的密钥"></script>
    <script>
        (function () {
            (function () {
            var map = new BMapGL.Map("container"); // 创建地图实例
            var point = new BMapGL.Point(116.404, 39.915); // 创建点坐标
            map.centerAndZoom(point, 15); // 初始化地图,设置中心点坐标和地图级别
        })();
        })();
    </script>
</body>
</html>
```

##### 注意事项
1. 配置容器大小
2. 需使用百度BD09坐标，如使用其他坐标（ WGS84、GCJ02）进行展示，需先将其他坐标转换为BD09

#### 进阶展示

##### 鼠标事件
```js
map.enableScrollWheelZoom(true); // 开启鼠标滚轮缩放
```

##### 旋转和倾斜角度
```js
map.setHeading(64.5);   //设置地图旋转角度
map.setTilt(73);       //设置地图的倾斜角度
```

##### 地图类型
+ 地球模式
`map.setMapType(BMAP_EARTH_MAP);`

+ 自定义地图样式
    1. [在线创建地图样式](http://lbsyun.baidu.com/index.php?title=open/custom)
    2. 发布并获取样式ID`3d71dc5a4ce6222d3396801dee06622d`/json样式`styleJson`
    3. 引入
    ```js
    map.setMapStyleV2({     
        styleId: '3d71dc5a4ce6222d3396801dee06622d'
        styleJson:styleJson
    });
    ```

##### 轨迹动画
1. 轨迹动画依赖开源库BMapGLLib.TrackAnimation,需要在页面当中引用轨迹动画开源库静态JS文件
`<script type="text/javascript" src="//api.map.baidu.com/library/TrackAnimation/src/TrackAnimation_min.js"></script>`
2. 创建基本地图
```js
var bmap = new BMapGL.Map("allmap");                          // 创建Map实例
bmap.centerAndZoom(new BMapGL.Point(116.297611, 40.047363), 17);    // 初始化地图，设置中心点坐标和地图级别
bmap.enableScrollWheelZoom(true);                             // 开启鼠标滚轮缩放
```
3. 自定义一个折线轨迹
```js
var path = [{
    'lng': 116.297611,
    'lat': 40.047363
}, {
    'lng': 116.302839,
    'lat': 40.048219
}, {
    'lng': 116.308301,
    'lat': 40.050566
}, {
    'lng': 116.305732,
    'lat': 40.054957
}, {
    'lng': 116.304754,
    'lat': 40.057953
}, {
    'lng': 116.306487,
    'lat': 40.058312
}, {
    'lng': 116.307223,
    'lat': 40.056379
}];
var point = [];
for (var i = 0; i < path.length; i++) {
    point.push(new BMapGL.Point(path[i].lng, path[i].lat));
}
var pl = new BMapGL.Polyline(point);
```
4. 创建个轨迹动画对象，并配置参数
轨迹动画的构造参数接受3个参数。参数1为当前地图对象；参数2为折线对象；参数3为配置对象
```js
var trackAni = new BMapGLLib.TrackAnimation(bmap, pl, {
    overallView: true, // 动画完成后自动调整视野到总览
    tilt: 30,          // 轨迹播放的角度，默认为55
    duration: 20000,   // 动画持续时长，默认为10000，单位ms
    delay: 3000        // 动画开始的延迟，默认0，单位ms
});
```
5. 启动动画
`trackAni.start();`
6. 强制停止动画
`trackAni.cancel(); // 强制停止动画`
###### 方法
+ start() 开始动画
+ cancel() 终止动画
+ setPolyline(polyline: Polyline) 设置动画轨迹折线覆盖物
+ getPolyline() 获取动画轨迹折线覆盖物
+ setDelay(delay: Number) 动画开始延迟，单位ms
+ getDelay() 获取动画开始延迟，单位ms
+ setDuration(duration: Number) 设置动画持续时间。建议根据轨迹长度调整，地图在轨迹播放过程中动态渲染，动画持续时间太短影响地图渲染效+ 果。
+ getDuration() 获取动画持续时间
+ enableOverallView() 开启动画结束后总览视图缩放（调整地图到能看到整个轨迹的视野），默认开启
+ disableOverallView() 关闭动画结束后总览视图缩放（调整地图到能看到整个轨迹的视野），默认关闭
+ setTilt(tilt: Number) 设置动画中的地图倾斜角度，默认55度
+ getTilt() 获取动画中的地图倾斜角度
+ setZoom(zoom: Number) 设置动画中的缩放级别，默认会根据轨迹情况调整到一个合适的级别
+ getZoom() 设置动画中的缩放级别