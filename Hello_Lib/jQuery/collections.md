[TOC]

# 记录使用jQuery中遇到的问题

## 动画

### 快速连续触发事件,动画滞后的反复执行
#### 问题: 快速连续点击触发`slideToggle`事件,动画滞后的反复执行

> jQuery中slideUp 、slideDown、animate等动画运用时，如果目标元素是被外部事件驱动, 当鼠标快速地连续触发外部元素事件, 动画会滞后的反复执行，其表现不雅。非常影响使用体验。

**解决思路**: 在触发元素的事件时预先停止所有的动画，再执行相应的动画事件（使用stop函数）。
```js
// 原代码
$(element).click(function () {
  $(this).slideToggle(500);
})

// 使用stop()改进代码
$(element).click(function () {
  $(this).stop().slideToggle(500);
})
```


## 方法api
### 为元素添加类名且移除兄弟元素类名
采用`siblings()`方法
```js
$(element).addClass('active').siblings().removeClass('active');
```
