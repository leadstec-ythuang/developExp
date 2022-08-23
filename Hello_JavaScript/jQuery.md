[TOC]
# 记录使用jQuery时的问题及探索与发现

## 探索与发现
### 循环
#### 1. jQuery跳出each循环的方法
开发中在each中有个需求是不满足条件时不执行循环下的语句，第一时间想到的是使用continue，发现报错了。
##### JQuery each循环,要实现break和continue的功能
break ---- 用returnfalse;
continue ---- 用return true;

##### on绑定动态创建元素事件
```js
  // 错误写法
  // 只为第一个element元素绑定了click事件，动态创建的则没有绑定
  $('element').on('click', function() {
    ...
  })


  // 正确写法
  $('body').on('click', 'element', function() {
    ...
  })

```