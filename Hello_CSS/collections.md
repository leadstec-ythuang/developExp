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

> 解决：width: max-content;


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
### 解决flex布局space-between尾部元素左对齐
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

