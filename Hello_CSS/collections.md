[TOC]

# 关于遇到的或知识模糊的样式属性

## 媒体资源
### object-fit
指定元素的内容适应指定容器的高度与宽度,一般用于img 和 video 标签

+ **fill** 默认，不保证保持原有比例，内容拉伸填充整个容器
+ **contain** 保持原有比例，内容缩放
+ **cover** 保持原有尺寸比例。但部分可能被剪切
+ **none** 保留原有元素内容的长和宽，即内容不会被重置
+ **scale-down** 保持原有尺寸比例。内容的尺寸与none或contain中的一个相同，取决于它们两个间谁得到的对象尺寸会更小一点


### 一些需求实现
1. 图片随屏幕缩放固定比例进行缩放

2. 同一行子元素高度以最高的子元素高度为基准进行同高处理


## 定位
### sticky
基于用户的滚动位置来定位。

粘性定位的元素是**依赖于用户的滚动**，在 position:relative 与 position:fixed 定位之间切换。

它的行为就像 position:relative; 而当页面滚动超出目标区域时，它的表现就像 position:fixed;，它会固定在目标位置。

元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。

这个特定阈值指的是 top, right, bottom 或 left 之一，换言之，指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

> 遇到的问题是有一个sticky的元素在遮罩弹窗弹出时sticky失效。

> 分析原因：页面滚动超出了sticky元素的目标区域，元素黏在页面顶部，但是点击弹出dialog遮罩时，滚动条转为dialog的元素，远比原先滚动的少，因此此时sticky元素的行为表现为position:relative

> 解决的方法：理论要回归sticky属性未出现前的使用js进行控制，判断元素的滚动距离进行定位

## 字体属性
### opacity

### 文字

#### 字体与图表垂直对齐
需要垂直居中对齐的元素加上`vertical-align: middle;`
#### li列表list-style-position为inside并且换行文字对齐实现
```css
  margin-left: 1.5em;
  text-indent: -1.5em;
```
## layout
#### 父元素宽度自适应子元素宽度之和
> 需求：swiper3里面wrapper的宽度等于实际slide宽度之和，让条件“wrapper宽度大于container宽度时初始化swiper”成立。

> 想法：让wrapper脱离文本流（脱离文本流的三种方式：float; absolute定位; fixed定位）

> 解决：width: max-content(ie不兼容);

### 
> 在移动端避免使用100vh
移动端浏览器（Chrome/Safari）有菜单栏/地址栏，地址栏有时可见有时隐藏，改变了视窗的大小。这些浏览器没有将100vh的高度调整为视口高度变化时屏幕的可见部分，而是将100vh设置为隐藏地址栏的浏览器高度。结果是当地址栏可见时，屏幕底部的部分被切断，从而破坏了100vh的初衷
## Hack
1. 自定义箭头
```css
&::before {
  border-left: 2px solid #fff;
  border-top: 2px solid #fff;
  content: '';
  display: block;
  opacity: 0.5;
  transition: 0.25s;
  width: 11px;
  height: 11px;
  position: absolute;
}

.swiper-button-prev {
  left: 40px;

  &::before {
    left: 17px;
    top: 15px;
    transform: rotate(-45deg);
  }
}

.swiper-button-next {
  right: 40px;

  &::before {
    left: 13px;
    top: 14px;
    transform: rotate(135deg);
  }
}
```

## 布局
### 盒模型(box model)
可以将所有HTML元素看做一个盒子。
+ Content（内容） - 由内容边界限制，容纳着元素的“真实”内容，例如文本、图像，或是一个视频播放器。
+ Padding（内边距） - 由内边距边界限制，扩展自内容区域，负责延伸内容区域的背景，填充元素中内容与边框的间距。当其取值为百分比时，该百分比都是相对于包含该元素的块的宽度（相对于该块的百分比）。
+ Border（边框） - 由边框边界限制，扩展自内边距区域，是容纳边框的区域。即围绕在内边距和内容外的边框。
+ Margin（外边距） - 由外边距边界限制，用空白区域扩展边框区域，以分开相邻的元素。当其取值为百分比时，该百分比都是相对于包含该元素的块的宽度（相对于该块的百分比）。

#### 标准盒模型
标准盒模型中 width 指的是内容（Content）区域的宽度；height 指的是内容（Content）区域的高度。标准盒模型下盒子的大小 = width + Border + Padding + Margin。
#### 怪异盒模型
怪异盒模型中的 width 指的是内容（Content）、边框（Border）、内边距（Padding）总和的宽度，即 width = Content + Border + Padding；height 指的是内容（Content）、边框（Border）、内边距（Padding）总和的高度。怪异盒模型下盒子的大小 =width + Margin。
#### 设置盒模型解析模式
box-sizing
  > + content-box： 默认值，border 和padding 不算到 width 范围内，可以理解为是 W3C 的标准模型
  > + border-box：border 和 padding 划归到 width 范围内，可以理解为是 IE 的怪异盒模型
  > + padding-box：将 padding 算入 width 范围
### CSS样式的百分比
#### 宽/高度百分比
子元素宽/高度百分比是基于父级元素的width/height，不包含padding和border
> 高度百分比一定要求父元素设置有height属性，只设置min-height虽然父元素会有高度，但子元素使用百分比无法继承
```css
.box {
	width: 400px;
	height: 200px;
	padding: 20px;
	background-color: red;
}

.item {
	width: 100%;
	height: 100%;
	background-color: blue;
}
```
```html
<div class="box">
    <div class="item">item元素的width为400px 高度为200px</div>
</div>
```
如果父级元素box-sizing: border-box，子级元素大小的百分比基于父级元素真正的大小，即除去padding，border之后的大小

```html
<div class="box">
    <div class="item">父级box的宽度被padding占用40px，item元素的宽度只剩360px</div>
</div>
```
#### 定位元素的宽高百分比
子级定位元素的宽/高100%=父级定位元素width/height+padding的大小
  > 这里继承的并非是直接的父元素，而是父级定位元素，如果直接父元素没有定位，则继续查找更上一级的父元素，直到找到有定位的父元素或者body为止

``` css
.box {
	width: 400px;
	height: 200px;
	padding: 20px;
	background-color: red;
	position: relative;
	border: 10px solid #ccc;
	margin: 50px;
}

.item {
	width: 100%;
	height: 100%;
	background-color: blue;
        position: absolute;
        top:0;
        left:0;
}
```
```html
<div class="box">
    <div class="item">item元素的width=440，item元素的height=240</div>
</div>
```
如果父元素是怪异盒模型，则父元素的width按实际的来计算
####  padding和margin百分比
子元素的padding和margin都是基于父元素的宽度
```css
#box{
    width: 500px;
}
#box > div{
    width: 300px;
    padding-left: 10%;
    margin-left: 10%;
}
<div id="box">
    <div></div>
</div>
```
```js
$(function() {
    console.log($('#box>div').css('paddingLeft'));//50px
    console.log($('#box>div').css('marginLeft'));//50px
})
```
#### line-height百分比
line-height设置百分比是基于当前字体大小，默认值为100%
```css
#box{
    font-size: 20px;
    line-height: 100%;
}

<div  id="box"></div>
```
```js
$(function(){      
    console.log($('#box').css('lineHeight'));//20px
})
```
### 解决flex布局space-between尾部元素左对齐
 span方案
## 偏
### clip-path
### background
```css
background: -webkit-gradient(linear, left top, left bottom, color-stop(0%, rgba(0,0,0,0)), color-stop(100%, rgba(0,0,0,0.8)));

background: -webkit-gradient(linear, left top, left bottom, from(rgba(1,1,1,0)), to(rgba(0,0,0,0.8)))

background: linear-gradient(to bottom, rgba(1,1,1,0), rgba(0,0,0,0.8));
```
### 列表样式
+ list-style-position-相对于对象的内容绘制列表项标记
  - inside
  + outside

