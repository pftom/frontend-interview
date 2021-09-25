- 下午笔试：
   - 用下午的时间做了一道算法题：[https://leetcode-cn.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/](https://leetcode-cn.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)
- 晚上面试：
   - 自我介绍
   - 对未来有什么规划
   - 了解我们团队吗？
   - 读过源码吗？
      - React 中的 Diff 算法说一说？
      - 为什么一开始是 O(n^3)
      - 之后怎么又优化到 O(n) 的？说一说过程
      - 底层的 key 比较是怎么样的？
      - 具体比较 key 的过程是在源码中的那个函数？
   - 听说你做过小程序，那你了解小程序底层渲染的机制吗？
   - 说一说你最近的项目：
      - 如何做性能优化的？
      - 关键渲染路径了解吗？
      - 性能优化中最重要的指标 LCP（Last content paint）知道吗？
      - 自己有统计具体优化的耗时吗？怎么统计？
   - 你还有哪些 offer？
      - 为什么有了 offer 还要面腾讯？你会怎么选择？
   - 做过 Node 开发吗？
      - 用过哪些服务器框架？
      - 讲 Express 搭建一个 API 服务器的过程？
      - 了解核心的路由怎么设置吗？
      - 常见的路由有哪些？
      - 如何设置 Cookie？
      - 前端如何修改 Cookie？怎么设置 Cookie？
         - 用过哪些请求库？
            - fetch 底层是如何封装的？
            - ajax 的监听调用完成的方法是哪个？



## 读过源码嘛？


### 为什么一开始是 O(n^3)？


首先需要对比找到旧节点在新树中的位置，这需要 O(n^2) ，然后找到差异之后，还需要计算最小的转换操作，这个需要遍历一次节点树，又需要 O(n)，所以需要 O(n^3)。


### [之后怎么又优化到 O(n) 的？说一说过程](https://www.yuque.com/xmh/avfn8z/gz8rcg#YsGBe)


使用了三种 Diff 策略：


- Tree Diff 
- Component Diff
- Element Diff



### [底层的 key 比较是怎么样的？](https://www.yuque.com/xmh/avfn8z/gz8rcg#YsGBe)


### 具体比较 key 的过程是在源码中的那个函数？


在 `ReactMultiChild.js` 里面：


- Tree Diff：在外面更新的是 `updateChildren` 函数，实际内部会调用 `_updateChildren` ，然后最后会调用 `processQueue` 和 `clearQueue` 
- Element Diff： `_updateChildren` ，实际更新过程， `moveChild` 、 `removeChild` 、 `createChild` 、 `_unmountChild` 、 `_mountChildAtIndex` 
   - 1）INSERT_MARKUP：enqueueInsertMarkup  
   - 2）MOVING_EXISTING：enqueueMove
   - 3）REMOVE_NODE：enqueueRemove





## 小程序底层渲染机制


- 提供了 JS 运行环境，由 JS 编写的业务代码完成逻辑层的处理
   - 在 JS Core 中运行
- 通过数据传输接口（注册时的 data 属性及后续的 setData 方法调用）将逻辑层的数据传递给视图层
- 视图层由 WXML 语言编写的模板通过该 “数据绑定” 与逻辑层传输过来的数据 merge 成展示结果并展现
   - 视图为纯 Native 渲染，所以在 Native 端
   - Native 端读取 WXML 模板文件，再根据逻辑层传来的数据以及对应的指令，生成最终展现内容的标签串
   - 再以解析 XML 的方式解析标签为带有属性的节点树，并对应节点树中各节点相应创建 Native 中的视图元素
- 视图的样式控制由  WXSS 语言编写的样式规则进行配置
   - 构建视图元素之后，仍由 Native 读取 WXSS 文件，用简单字符串匹配即可解析选择器和规则，然后设置对应的属性值



### 参考资料


- [https://www.zhihu.com/question/50920642](https://www.zhihu.com/question/50920642)



## 说一说你最近的项目：


### [如何做性能优化的？](https://www.yuque.com/xmh/avfn8z/wrzl64#l6uYg)


- [前端性能优化清单系列](https://mp.weixin.qq.com/s?__biz=MzI5NjIzNjA1Nw==&mid=2247484177&idx=1&sn=bbc61b91a180bd2670233bf958afb9fc&scene=21#wechat_redirect)
### [关键渲染路径了解吗？](https://www.yuque.com/xmh/avfn8z/wrzl64#l6uYg)


### 性能优化中最重要的指标 LCP（Largest content paint）知道吗？


可以测试到用户感知页面加载的速度，当页面的主要内容加载完成时，它记录下这个时间点，一个快速的 LCP，可以让用户感受到这个页面的可用性：


- 是视窗最大可见图片或者文本块的渲染时间（一般至少是 75% 的用户能够在 2.5 秒内，包括移动和桌面设备）
- 主要包含： `img` 、 `svg` 中的 `image` 、 `video` 、 带 `url` 背景图的元素、块级元素带有文本节点或者内嵌文本子元素
- 如何上报：统计最近一次上报的 `PerformanceEntry` 即可，这就是 LCP
- 如何测量：使用 `Element Timing API` 或者 `web-vitals` 库
- 如何改善：[如何做性能优化？](https://www.yuque.com/xmh/avfn8z/wrzl64#l6uYg)



#### 参考链接


- [https://juejin.im/post/6858636720561520654](https://juejin.im/post/6858636720561520654)



### 自己有统计具体优化的耗时吗？怎么统计？


引入通过前端监控的知识：[https://www.yuque.com/xmh/avfn8z/st7emq#p4GNf](https://www.yuque.com/xmh/avfn8z/st7emq#p4GNf)


## 做过 Node 开发吗？


### 用过哪些服务器框架？
Express


### 讲 Express 搭建一个 API 服务器的过程？
```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => {
  console.log(`Example app listening at http://localhost:${port}`)
});
```


### 了解核心的路由怎么设置吗？


- `app.METHOD(PATH,HANDLER)` 
- `app.get` 
- `app.post` 
- `app.put` 
- `app.delete` 



### 常见的路由有哪些？


- get
- post
- put
- delete
- options
- trace



## 如何设置 Cookie？


### Cookie 是什么？
Cookie 是一个键值对，是存储在浏览器中的纯文本。


- 跟随域名请求
- 最大 4KB，单域名最多 20 个 Cookie
- 适合存储用户身份认证信息
- 可以通过设置 HttpOnly 来禁止 JS 访问
- 以字符串的形式存储，通过分号+空格分隔
```javascript
document.cookie = "userId=3013; userName=pftom"
```
### Cookie 的流转过程


在 Chrome 开发者工具里面的 Resources 栏，选择 Cookies，可以看到设置的 Cookie，同时可以看到所属的域，流程如下：


- 设置 Cookie
- Cookie 自动添加到 Request Header 中
- 服务端接收到 Cookie



### Cookie 的格式


- expires
- domain
- path
- secure：只有在确保安全的情况下才会发送，比如 HTTPS 请求，只能在 HTTPS 协议的网页中才能直接通过 JS 去设置 `secure` 属性
- HttpOnly



### 怎么设置 Cookie？


服务器端和客户端都可以设置 Cookie：


- 服务器端如何设置？
   - 通过服务器端响应返回的 response 的 header 中的 `set-cookie` 字段来设置，多个 cookie，需要有多个 `set-cookie` ，它的值是一个字符串
- 客户端如何设置？
   - 可以设置 `expires` 、 `domain` 、 `path` 、 `secure` （在 HTTPS 网页中），不能设置 `HttpOnly` 
   - 通过 `document.cookie` 来设置，执行多次来设置多个 Cookie，单纯执行一次，里面多个键值对无法设置成功
```javascript
document.cookie = "name=Jonh";
document.cookie = "age=12";
document.cookie = "class=111";
```
### 如何修改、删除


- 重新赋值
   - 注意：其他几个选项要保持一致，如  `path` 、 `domain` 
- 重启赋值，设置 `expires` 为过去的时间
   - 注意：其他几个选项要保持一致，如  `path` 、 `domain`
### Cookie 编码


- 通过 `escape/unescape` 来做
- 通过 `encodeURIComponent/decodeURIComponent` 来做
- 通过 `encodeURI/decodeURI` 



```javascript
var key = escape("name;value");
var value = escape("this is a value contain , and ;");
document.cookie= key + "=" + value + "; expires=Thu, 26 Feb 2116 11:50:25 GMT; domain=sankuai.com; path=/";
```


### 跨域请求中的 Cookie


设置 xhr.withCredentials 为 true，然后服务器端允许跨域请求：


- 设置 Access-Control-Allow-Credentials: true，这样浏览器自动将 cookie 加在 request header
- 而且 server 端要设置 Access-Control-Allow-Origin 为请求页面的域名不能是 *



## 你了解 XMLHttpRequest 嘛？


> 使用 XMLHttpRequest 对象来发送 Ajax 请求，Ajax 请求实际上是一个 HTTP 请求



### 如何使用？


```javascript
function sendAjax() {
  var formData = new FormData();
  formData.append('username', 'johndoe');
  formData.append('id', 123456);
  
  // 创建 XHR 对象
  var xhr = new XMLHttpRequest();
  
  // 设置 xhr 请求的超时时间
  xhr.timeout = 3000;
  
  // 设置响应返回的数据格式
  xhr.responseType = 'text';
  
  // 创建一个 POST 请求，采用异步
  xhr.open('POST', '/server', true)
  
  // 注册相关事件的回调处理函数
  xhr.onload = function (e) {
    if (this.status == 200 || this.status == 304) {
      alert(this.reponseText);
    }
  }
  
  xhr.ontimeout = function (e) { ... }
  xhr.onerror = function (e) { ... }
  xhr.upload.onprogress = function (e) { ... }
  
  // 发送数据
  xhr.send(formData);                                       
}
```


### 如何设置 Header 和获取 Response Header


- setRequestHeader：在 xhr.open 和 xhr.send 之间使用，否则会报错
- getAllResponseHeaders 和 getResponseHeader 只能获取到 safe 的 header字段
   - getReponseHeader 中获取 header 时，如果存在跨域可以获取到 `Access-Control-Expose-Headers` 



### 如何获取 response 数据


- xhr.response
- xhr.responseText
- xhr.responseXML



### 如何追踪 ajax 请求的当前状态


- xhr.readyState 可以追踪，只读，有五个值，每次变化都会触发 xhr.onreadystatechange
   - 0: UNSET
   - 1: OPENED
   - 2: HEADERS_RECEIVED
   - 3: LOADING
   - 4: DONE



### 如何设置超时时间


- xhr.timeout 可以在 xhr.send 之前或者之后，但是都是从 xhr.send 开始算，且只能异步
- xhr.onloadstart 开始 xhr.loadend 结束



### 如何注册成功的回调


在 `xhr.onload` 里面比较好， `xhr.onreadystatechange` 里面可能会调用多次


- [https://www.w3ctech.com/topic/854](https://www.w3ctech.com/topic/854)
- [https://segmentfault.com/a/1190000004322487#comment-area](https://segmentfault.com/a/1190000004322487#comment-area)



## 你了解 Fetch 嘛？


### 为何要使用 Fetch：


- XHR 将输入、输出、和用事件来跟踪的状态混杂在一个对象里，不符合职责分离的原则
- 基于事件的模型与最近的 JS 流行的 Promise 以及生成器的异步编程模型不太搭



### 带来的优化


- 改善离线体验
- 保证可扩展性



### 简单示例
```javascript
// GET 请求
fetch('/data.json').then(function (res) {
  if (res.ok) {
    res.json().then(function (data) {
      console.log(data.entries);
    });
  } else {
    console.log("Looks like the response wasn't perfect, got status", res.status);
  }
}, function (e) {
  console.log('Fetch failed!', e);
});
```
```javascript
// POST 请求
fetch('http://www.example.org/submit.php', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/x-www-form-urlencoded'
  },
  body: "firstName=Nikhil&favColor=blue&password=easytoguess",
}).then(function (res) {
  if (res.ok) {
    alert('Perfect! Your settings are saved.');
  } else if (res.status === 401) {
    alert('Oops! You are not authorized.');
  }
}, function (e) {
  alert('Error submitting form!');
});
```
### 三大接口


- Request：
   - URL
   - method
   - headers: Header
   - { mode: 'same-origin' } 或者 `cors` 、 `no-cors` 
   - { credentials: 'omit' } 或者 `same-origin` 、 `include` 
   - referrer 等
- Response
   - status：状态码
   - statusText：默认值是 `"OK"` 
   - ok：在 status 为 200 的时候为 true
   - headers: Header 只读
   - 静态方法：Response.error() 简单返回一个错误的请求、Response.redirect(url, status) 跳转 URL 请求
- Header：
   - `Content-Type` 
   - `Content-Length` 
   - `X-Custim-Header` 
   - `Set-Cookie` 
   - guard: 决定哪些是可写的，没有暴露给 Web
      - none
      - request
      - request-no-cors
      - response
      - immutable



### 参考链接

- [https://www.w3ctech.com/topic/854](https://www.w3ctech.com/topic/854)
## 什么是 CORS ？


> 整个 CORS 通信的过程都是浏览器自动完成的，浏览器会额外添加一些头信息或者附加额外的请求。所以实现关键是服务器端实现了 CORS 接口。



包含两类请求：


- 简单请求
- 非简单请求



### 简单请求
#### 流程：

- 客户端在请求头加上 Origin 字段，标志来源域
- 服务器端根据这个字段来确定源是否在许可范围内
   - 如果不在许可范围内，返回一个正常的 HTTP 响应，响应头信息没有 `Access-Control-Allow-Origin` ，客户端收到之后，发现响应头没有这个字段，抛出错误，走 `XMLHttpRequest` 的 `onerror` 函数，注意，这种错误无法通过状态码识别
   - 如果再许可范围内，服务器端返回的响应会多出几个头信息字段：
      - `Access-Control-Allow-Origin` ：必须，要么是 Origin 字段的值，要么是 *
      - `Access-Control-Allow-Credentials` ：是否允许客户端发送请求时带上 Cookie，服务器端要么设置为 `true` 要么不设置这个字段，相应的客户端的 xhr 要打开 `xhr.withCredentials = true` 
         - 如果设置了这个选项， `Access-Control-Allow-Origin` 只能设置为和请求头 `Origin` 一样的域名
         - 遵循同源策略，只有用服务器域名设置的 Cookie 才会上传
         - 且因跨域限制，原网页中的 `document.cookie` 也无法读取服务器域名下的 Cookie
      - `Access-Control-Expose-Headers` ：添加的字段，可以允许 xhr.getResponseHeader 获取到对应字段的值
### 非简单请求


- 请求方法为 `PUT` 或者 `DELETE`  等
- `Content-Type` 字段的类型为 `applicaton/json` 



#### 流程：


- 浏览器端发送预检请求（preflight）：先询问服务器当前所在的域名是否在许可名单之中，以及可以使用那些 HTTP 动词和头信息字段，只有得到肯定答复，浏览器才会发出 `XMLHttpRequest` 请求，否则报错
   - 通过 `OPTIONS` 方法发送，用于询问，在头信息里面携带 `Origin` 、 `Access-Control-Request-Method` 、 `Access-Control-Request-Headers` 
- 服务器端收到之后，检查三个字段，然后返回预检请求的响应：
   - `Access-Control-Allow-Origin` 可以是 `Origin` 或者 `*` ，如果否定了预检请求，就会返回正常的 HTTP 响应，没有任何 CORS 相关的头信息字段，会触发 `XMLHttpRequest` 对象的 `onerror` 回调函数。
   - `Access-Control-Allow-Methods` ：所有支持的方法
   - `Access-Control-Allow-Headers` ：包括但不限于客户端的 `Access-Control-Allow-Headers` 
   - `Access-Control-Allow-Credentials` ：和简单请求一致
   - `Access-Control-Max-Age: 1728000` : 预检请求的有效期
- 服务器通过了 “预检” 请求之后，以后的每次 CORS 请求都和简单请求一样



### 与 JSONP 的比较


- 使用目的相同，但是比 JSONP 更强大
- JSONP 只支持 `GET` 请求，但是支持老式浏览器，也可以向不支持 CORS 的网站请求数据
- CORS 支持所有类型的 `HTTP` 请求
### 
### 参考资料


- [https://segmentfault.com/a/1190000004556040](https://segmentfault.com/a/1190000004556040)
- [https://www.ruanyifeng.com/blog/2016/04/cors.html](https://www.ruanyifeng.com/blog/2016/04/cors.html)



   - 用过哪些请求库？
      - fetch 底层是如何封装的？
      - ajax 的监听调用完成的方法是哪个？
