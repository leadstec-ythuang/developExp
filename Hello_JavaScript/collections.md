[TOC]

# 记录开发过程中遇到的js问题

## 兼容性问题
### IE兼容
1. let关键字
  + IE10及之前的版本都不兼容
  + IE11兼容但是index不单独绑定到for循环的每个迭代

> 遇到的问题
```js
// 每次点击输出循环最后的index值
docLabels = document.getElementsByClassName('doc-label');
docCheckboxs = document.getElementsByClassName('doc-checkbox');

for (let labelIndex = 0; labelIndex < docLabels.length; labelIndex++) {
    const docLabel = docLabels[labelIndex];
    docLabel.onclick = function () {
      console.log("index: " + labelIndex);
      console.log("docCheckboxs[index]: " + docCheckboxs[labelIndex]);
    }
}
```
> 分析：由于js没有块级作用域，for循环的每个loop共享同一个上下文环境，与onclick事件绑定的匿名函数内部不存在变量index，因此引用上下文环境中的变量index，等待onclick被触发，for循环执行完毕，外层index的值已为docLabels.length。
> 解决：
```js
docLabels = document.getElementsByClassName('doc-label');
docCheckboxs = document.getElementsByClassName('doc-checkbox');

// 1. 再加一层闭包
for (let labelIndex = 0; labelIndex < docLabels.length; labelIndex++) {
    const docLabel = docLabels[labelIndex];
    (function (j) {
      docLabel.onclick = function () {
      console.log("index: " + j);
      console.log("docCheckboxs[index]: " + docCheckboxs[j]);
    }
    })(labelIndex)
}

// 2. 将循环的index值作为对象属性保存
for (let labelIndex = 0; labelIndex < docLabels.length; labelIndex++) {
    const docLabel = docLabels[labelIndex];
    docLabel.index = labelIndex;
    docLabel.onclick = function () {
      console.log("index: " + this.index);
      console.log("docCheckboxs[index]: " + docCheckboxs[this.index]);
    }
}
```


## 媒体查询
```js
//创建查询列表

let mediaSqls = [
  window.matchMedia('(max-width: 768px)'),
  window.matchMedia('(max-width: 1024px)'),
  window.matchMedia('(max-width: 1200px)')
]

//定义回调函数
function mediaMatches () {
  if(mediaSqls[0].matches) {
    ...
  } else if(mediaSqls[1].matches) {
    ...
  } else if(mediaSqls[2].matches) {
    ...
  } else {
    ...
  }
}

//首次加载
mediaMatches();

for(let i = 0; i < mediaSqls.length; i++) {
  mediaSqls[i].addListener(mediaMatches);
}
```
## usage

1. extend



  $('.es-btn-group-container .btn-group-faq-dialog .dialog-question').click(function (e) { 
    e.preventDefault();
    $(this).toggleClass('active');
    $(this).next().toggle();
    $('.es-btn-group-container .btn-group-dialog .dialog-container').getNiceScroll().onResize();
  });