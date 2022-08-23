[TOC]
## 起源
传统开发模式过多操作界面细节，比如需要掌握和使用大量DOM(Document Object Model)的API(方法，属性)；数据(状态)分散，不方便管理和维护
思考：
  + 以组件方式划分功能模块
  + 组件内以jsx(Javascript和XML结合的一种格式)来描述UI的样子，以state来存储组件内的状态
  + 当应用的状态改变时，通过setState来修改状态，状态发生变化时，UI自动更新
### 
## 开发依赖
+ react：包含react所必须得核心代码
+ react-dom：react渲染在不同平台所需要的核心代码
+ babel：将jsx转换成React代码的工具