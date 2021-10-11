[TOC]

# 关于遇到的或知识模糊的样式属性

## 媒体资源标签属性
### object-fit
指定元素的内容适应指定容器的高度与宽度,一般用于img 和 video 标签

+ **fill** 默认，不保证保持原有比例，内容拉伸填充整个容器
+ **contain** 保持原有比例，内容缩放
+ **cover** 保持原有尺寸比例。但部分可能被剪切
+ **none** 保留原有元素内容的长和宽，即内容不会被重置
+ **scale-down** 保持原有尺寸比例。内容的尺寸与none或contain中的一个相同，取决于它们两个间谁得到的对象尺寸会更小一点


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
## 偏
### clip-path
