[TOC]
https://blog.csdn.net/cw_hello1/article/details/121540023
https://blog.csdn.net/jingdianjiuchan/article/details/129727122
https://www.cnblogs.com/hezemin/p/16830868.html
https://blog.csdn.net/m0_65121454/article/details/129787472
# iframe与跨域通信

## 跨域
在浏览器地址栏输入一个地址时,这个地址通常包含四部分信息内容包括: 1.协议,2.域名,3.端口,4.资源位置.
其中前三部分即协议,域名,端口存在一个不一样,浏览器就会认为存在跨域的问题
> 例如: 

| 域名1 | 域名2 | 是否跨域 |
| ---- | ---- | ---- |
| http://father.moxiao.com/a/a.html | http://father.moxiao.com/b/b.html | 不存在跨域 |
| https://father.moxiao.com/a/a.html | http://father.moxiao.com/b/b.html | 存在跨域，协议不一样 |
| http://father.moxiao.com/a/a.html	| http://child.moxiao.com/b/b.html | 存在跨域，域名不一样 |
| http://father.moxiao.com/a/a.html	| http://child.moxiao.com:8082/b/b.html	| 存在跨域，端口不一样 |

### 跨域的种类
1. 使用XMLHttpRequest或者使用AJAX发送的POST或者GET请求.这种形式的跨域是: **前端**和**后端**进行跨域请求(这里不讨论)
2. 父子页面之间进行DOM操作(父子窗口之间的document操作),这种形式的跨域是:前端页面与前端页面之间的通信或者相互操作形成的跨域

### 跨域的提示
如果在页面