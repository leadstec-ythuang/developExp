[TOC]
# 使用swiper过程遇到的问题

### 在整屏的情况下，ios端autoplay时出现闪屏
解决方案：在出现闪烁的元素加`transform: translate3d(0, 0, 0)`

    `transform: translate3d(0, 0, 0)`的作用：使某些设备运行其硬件加速。其不会执行任何操作，它将对象沿x，y，z轴移动0px，只是强制硬件加速的一项技术。
    另一种选择是`-webkit-transform: translateZ(0)`。如果Chrome和Safari由于转换而闪烁，请尝试`-webkit- backface-visibility: hidden`和`-webkit-perspective: 1000`。