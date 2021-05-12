[TOC]

# 记录开发过程中遇到的CSS问题及解答

## 滑动

+ Q: dialog遮罩后面的页面仍然可以滑动
+ A: 在dialog显示的时候设置body的overflow为hidden;


## 选择器
+ Q: nth-child和nth-of-type的区别
+ A: nth-child根据元素的个数进行计算,忽略元素类别
     nth-of-type按照类型进行计算
