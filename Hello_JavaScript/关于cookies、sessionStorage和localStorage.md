[TOC]
# 关于cookies、sessionStorage和localStorage解释及区别

## cookie
cookie是客户端与服务器端进行绘画使用的一个能够在浏览器本地化存储的技术.简言之,cookie是服务器端发给客户端的文本文件,但只能存储4kb的数据;目的是用于辨别用户身份,记录跟踪购物车的商品信息(如数量),记录用户访问次数等
> cookie的内容主要包括:名字(name),值(value),过期时间(expires),路径path和域domain.路径和域一起构成cookie的作用范围.一般cookie储存在内存中,若设置了过期时间则储存在硬盘里,浏览器页面关闭也不会失效,直到设置的过期时间后才失效.若**不设置cookie的过期时间,则有效期为浏览器窗口的会话期间,关闭浏览器窗口就失效**

### 原理
客户端请求服务器时,如果服务器需要记录该用户状态,就使用response向客户端浏览器颁发一个cookie,而客户端会把cookie保存起来.当浏览器再请求服务器时,浏览器把请求的网址连同该cookie一同提交给服务器,服务器通过检查该cookie来获取用户状态

### 操作
#### 1. 设置cookie
```js
/*
  * set cookie
  * @param {string} category
  * @param {string || number} value cookie value
  * @param {number} expiresTime cookie expire time
  */
function setCookie(category, value, expiresTime) {
    let cname = DISABLED_TRAFFIC_LIGHT_AUTO_POPUP + category;
    let date = new Date();
    let dateTime = date.getTime();//获取当前时间
    date.setTime(dateTime + expiresTime);//设置过期时间，单位为天
    let expires = "expires=" + date.toUTCString();//拼接世界时间的过期时间（有效期数据），北京时间为当前时间加八小时即可
    document.cookie = cname + "=" + value + ";" + expires;//设置cookie数据和过期时间，中间用分号分隔
}
```

##### 遇过的问题1: 设置的cookie只有本路径和子路径能访问到,父级路径没有共享这个cookie
**问题所在是关乎path设置**

> **cookie path的默认值**
>  1. 当cookie的path设置了值不为null的时候,以设置的值为准
>  2. 当cookie的path为null的时候,获取请求的URI的path值
>    + 当URI的path值是以'/'结尾的时候,直接设置为cookie的path值
>    + 当URI的path值不是以'/'结尾的时候,查看path里面是否有'/'
>      - 如果有'/'的话,直接截取到最后一个'/',然后设置为cookie的path值
>      - 如果没有'/'的话,将cookie的path设置为'/'

**有路径的:**
https://www.baidu.com/test/a.html
如果设置cookie没有设置path,那么path的默认值为'/test'

**无路径的:**
 https://www.baidu.com/a.html
 如果设置cookie没有设置path，那么path的默认值为 "/"

因此希望在全站共享cookie需要在set cookie的时候加上path='/'
`document.cookie = cname + "=" + value + ";" + expires + ";path=/";`

#### 获取cookie
第一种:
```js
/*
* get cookie
* @param {string} cname cookie name
* @returns 
*/
function getCookie(cname) {
  const name = cname;
  const cookiesArr = document.cookie.split(";");
  for (let i = 0; i < cookiesArr.length; i++) {
      let cookiePair = cookiesArr[i].split('=');
      if (name == cookiePair[0].trim()) {
          return decodeURIComponent(cookiePair[1]);
      }
  }
  return null;
}
```
第二种:
```js
/*
* get cookie
* @param {string} cname cookie name
* @returns 
*/
function getCookie(cname) {
  const name = cname + "=";
  const ca = document.cookie.split(";");
  for (let i = 0; i < ca.length; i++) {
      const c = ca[i].trim();
      if (c.indexOf(name) === 0) return c.substring(name.length, c.length);
  }
  return "";
}
```

#### 移除cookie
```js
/*
* remove cookie
* @param {string} cname 
*/
function removeCookie(cname) {
  setCookie(key, '', -1);
}
```

## Web storage
> web storage的概念和cookie相似,区别在于web storage能够储存更多的数据,cookie的大小时受限制的,并且每次请求一个新的页面的时候都会被发送过去,无形中浪费了带宽,另外cookie还需要指定作用域,不可跨域调用

HTML5新增了两个储存方式:localStorage和sessionStorage

### localStorage
> localStorage的生命周期是永久,除非手动清除.储存的大小时5MB,仅在客户端浏览器上储存,不参与服务器通信.

#### localStorage的使用
localStorage通常以键值对的方式储存,通常储存为字符串
```js
//设置localStorage保存到本地，第一个为变量名，第二个是值
localStorage.setItem('Author', 'Jane')
// 获取数据
localStorage.getItem('Author')
// 删除保存的数据
localStorage.removeItem('Author')
// 清除所有保存的数据
localStorage.clear()
```

### sessionStorage的使用
> sessionStorage在当前会话下有效,引入了一个"浏览器窗口"的概念,sessionStorage是在同源的同窗口中,始终存在的数据,只要浏览器标签不关闭,刷新(即使是强制刷新)或者进入同源的另一个页面,数据仍存在.
同时打开"独立"的窗口,即使是同一个页面,sessionStorage的对象也是不同的,关闭窗口后sessionStorage就会被销毁

```js
// 设置sessionStorage保存到本地，第一个为变量名，第二个是值
sessionStorage.setItem('sessionName', 'John')
// 获取数据
sessionStorage.getItem('sessionName')
// 删除保存的数据
sessionStorage.removeItem('sessionName')
// 清除所有保存的数据
sessionStorage.clear()
```

### 复杂数据类型储存
> webStorage是以字符串的格式储存的,所以不能直接储存对象和数组类型,需要先转换一下格式在进行储存.

使用`JSON.stringify()`将对象转换成字符串
解析的时候,再使用`JSON.parse()`转换回去使用
### web Storage支持事件通知机制
> Web Storage API内建了一套事件通知机制，当存储区域的内容发生改变（包括增加、修改、删除数据）时，就会自动触发 storage 事件，并把它发送给所有感兴趣的监听者。因此，如果需要跟踪存储区域的改变，就需要在关心存储区域内容的页面监听 storage 事件

**`window.addEventListener("storage", handleStorage, false);`**

handleStorage 回调函数接受一个 StorageEvent 参数，在IE中，StorageEvent对象保存在 window.event 里面。

#### StorageEvent对象的属性及含义
+ key 设置或删除或修改的键.调用clear()时,则为null
+ oldValue 改变之前的旧值.如果是新增元素,则为null
+ newValue 改变之后的新值.如果是删除元素,则为null
+ storageArea 该属性是一个引用,指向发生变化的sessionStorage或localStorage对象
+ url 触发这个改变时间的页面的URL

### web Storage带来的好处
1. 减少网络流量:一旦数据保存在本地之后,就可以避免再向服务器请求,因此减少不必要的数据请求,减少数据在浏览器和服务器间不必要的来回传递
2. 快速显示数据: 性能好,从本地毒数据比通过网络从服务器上获得数据快得多,本地数据可以及时获得,再加上网页本身也可以有缓存,因此整个页面和数据都在本地的话,可以立即显示
3. 临时存储: 很多时候数据只需要在用户浏览一组页面期间使用,关闭窗口后数据就可以丢弃了,这种情况使用sessionStorage非常方便

## sessionStorage、localStorage和cookie的区别
### 共同点: 都是保存在浏览器端,且同源的

### 区别:
1. cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递，而sessionStorage和localStorage不会自动把数据发送给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下 
2、存储大小限制也不同，cookie数据不能超过4K，同时因为每次http请求都会携带cookie、所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大 
3、数据有效期不同，sessionStorage：仅在当前浏览器窗口关闭之前有效；localStorage：始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie：只在设置的cookie过期时间之前有效，即使窗口关闭或浏览器关闭 
4、作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localstorage在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的 
5、web Storage支持事件通知机制，可以将数据更新的通知发送给监听者 
6、web Storage的api接口使用更方便
