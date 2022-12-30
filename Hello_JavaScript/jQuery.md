[TOC]
# 记录使用jQuery时的问题及探索与发现

## 探索与发现
### 选择器
#### 获取兄弟元素
1. 前一个兄弟元素`prev()`;
   前一个类名为btn的兄弟元素`prev('.btn')`
2. 当前元素前面的所有兄弟元素`prevAll()`;
   当前元素前面类名为btn的所有兄弟元素`prev('.btn')`
3. 后一个兄弟元素`next()`;
   后一个类名为btn的兄弟元素`next('.btn')`
4. 后一个兄弟元素`nextAll()`;
   当前元素后面类名为btn的所有兄弟元素`nextAll('.btn')`
5. 获取当前元素的所有兄弟元素`siblings()`
   获取类名为btn的所有兄弟元素`siblings('.btn')`
### 循环
#### 1. jQuery跳出each循环的方法
开发中在each中有个需求是不满足条件时不执行循环下的语句，第一时间想到的是使用continue，发现报错了。
##### JQuery each循环,要实现break和continue的功能
break ---- 用`return false`;
continue ---- 用`return true`;

### 事件
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

#### 点击除指定元素外其他元素触发事件
##### closest()方法
返回被选元素的第一个祖先元素。该方法从当前元素向上遍历，直至文档根元素的所有路径(<html>),来查找匹配所传递的表达式的DOM元素的第一个祖先元素。

与parents()方法不同的地方：closest()从当前元素开始向上遍历，parents()方法从父元素开始。
```js
$(document).bind('click', function(e) {
  // target object
  var obj = $(e.target);
  if(obj.closest('#srdiv').length == 0) {// 点击除id为srdiv的元素
    ...
  }
})
```

#### hover()方法
`$(selector).hover(inFunction,outFunction)`
等同于
`$( selector ).mouseenter( handlerIn ).mouseleave( handlerOut );`