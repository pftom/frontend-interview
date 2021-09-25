- 自我介绍一下
- 之前在社交质量部门实习，为什么要来做前端？
- 聊项目
   - 做的什么的东西？
   - Slate 是自己写的，还是一个现成的编辑器
   - JS 对象树来进行渲染，主要的性能瓶颈在这里
   - 如何做性能优化？
      - 每说一步、问一步
- Promise 完全实现，需要链式调用
- 实现图片懒加载
- 对 CDN 有了解嘛？为什么会去离用户最近的地方查询？和 DNS 有关系吗？
- React 最新的生命周期说一下
   - 为什么要创造新的生命周期了？
   - 组件上面的 key 是干什么的？如何 key 发生了变化 React 会如何做？
   - 

- 单个并发限制异步
- Limit 的并发异步限制
- CSS 的标签优先级
- 说一说平时如何做性能优化？
   - SSR（服务器端渲染）
   - 首屏加载时，SSR 为什么会比客户端渲染要快
   - SSR 如何保证前后端拿到的数据 API 是一致的
- 智力题：
   - 某人在户外有一个烤肉架,恰好可容纳2片烤肉,肉需要正反两面烤熟才能吃，烤熟一面需要10分钟，问:怎样才能再最短时间内烤完三片肉.
- 说一说 HTTP 缓存？
- 能说一说 GC（垃圾收集器）的原理嘛？
- 手里还有那些 Offer？为什么选择 IVWeb，IMWeb 有了解过嘛，字节没有大的前端团队？



## Promise 完全实现，需要链式调用


> 经验：不要相信一些面试题总结大全里面的答案，基本上面试派不上用场，都是错的



```javascript
class MyPromise {
  constructor(fn) {
    this.callbacks = [];
    this.state = "PENDING";
    this.value = null;

    fn(this._resolve.bind(this), this._reject.bind(this));
  }

  then(onFulfilled, onRejected) {
    return new MyPromise((resolve, reject) =>
      this._handle({
        onFulfilled: onFulfilled || null,
        onRejected: onRejected || null,
        resolve,
        reject,
      })
    );
  }

  catch(onRejected) {
    return this.then(null, onRejected);
  }

  _handle(callback) {
    if (this.state === "PENDING") {
      this.callbacks.push(callback);

      return;
    }

    let cb =
      this.state === "FULFILLED" ? callback.onFulfilled : callback.onRejected;
    if (!cb) {
      cb = this.state === "FULFILLED" ? callback.resolve : callback.reject;
      cb(this.value);

      return;
    }

    let ret;

    try {
      ret = cb(this.value);
      cb = this.state === "FULFILLED" ? callback.resolve : callback.reject;
    } catch (error) {
      ret = error;
      cb = callback.reject;
    } finally {
      cb(ret);
    }
  }

  _resolve(value) {
    if (value && (typeof value === "object" || typeof value === "function")) {
      let then = value.then;

      if (typeof then === "function") {
        then.call(value, this._resolve.bind(this), this._reject.bind(this));

        return;
      }
    }

    this.state === "FULFILLED";
    this.value = value;
    this.callbacks.forEach((fn) => this._handle(fn));
  }

  _reject(error) {
    this.state === "REJECTED";
    this.value = error;
    this.callbacks.forEach((fn) => this._handle(fn));
  }
}

const p1 = new Promise(function (resolve, reject) {
  setTimeout(() => reject(new Error("fail")), 3000);
});

const p2 = new Promise(function (resolve, reject) {
  setTimeout(() => resolve(p1), 1000);
});

p2.then((result) => console.log(result)).catch((error) => console.log(error));
```
## 实现图片懒加载


将 src 设置为空或者一个默认的小图片、或者 loading 图片地址，然后在 data-src 上放置真实的地址，然后监听滚动事件，等到图片即将出现在视口的时候，就加载图片，这样便实现了懒加载：


```html
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        img {
            display: block;
            margin-bottom: 50px;
            width: 400px;
            height: 400px;
        }
    </style>
</head>

<body>

    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww1.sinaimg.cn/large/006y8mN6gw1fa7kaed2hpj30sg0l9q54.jpg" alt="">
    <img src="default.jpg" data-src="http://ww1.sinaimg.cn/large/006y8mN6gw1fa7kaed2hpj30sg0l9q54.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww1.sinaimg.cn/large/006y8mN6gw1fa7kaed2hpj30sg0l9q54.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">
    <img src="default.jpg" data-src="http://ww4.sinaimg.cn/large/006y8mN6gw1fa5obmqrmvj305k05k3yh.jpg" alt="">

</body>

<script>
  // 节流版本的懒加载
  function throttle(func, threshold) {
  let startTime = +new Date();
  let timer;

  return function () {
    let args = arguments,
      context = this,
      curTime = +new Date();

    clearTimeout(timer);

    if (curTime - startTime >= threshold) {
      func.apply(context, args);
    } else {
      timer = setTimeout(function () {
        func.apply(context, args);
      }, threshold);
    }
  };
}

let imgNodeArr = document.getElementsByTagName("img");
let len = imgNodeArr.length;
let n = 0;

lazyload();

window.addEventListener("scroll", lazyload);

function lazyload() {
  let clientHeight = document.documentElement.clientHeight;
  let scrollTop = document.documentElement.scrollTop || document.body.scrollTop;

  for (let i = n; i < len; i++) {
    if (imgNodeArr[i].offsetTop < clientHeight + scrollTo) {
      if (imgNodeArr[i].getAttribute("src") === "default.jpg") {
        imgNodeArr[i].src = imgNodeArr.getAttribute("data-src");
      }

      n = i + 1;
    }
  }
}

</script>
```


- [https://juejin.im/post/6844903455048335368](https://juejin.im/post/6844903455048335368)
## 对 CDN 有了解嘛？为什么会去离用户最近的地方查询？和 DNS 有关系吗？


CDN （内容分发网络），将源站的内容分发到里用户最近网络的边缘，使得用户可以就近获取数据，不仅降低了网络的拥塞状态，提高了响应速度，还可以减轻源站的负载压力。


- 本地 DNS 系统解析，查找缓存
- 本地没找到，找到 CDN 的 DNS 服务器
- 返回全局负载均衡的设备 IP 地址返回给用户
- 用户向 CDN 的全局负载均衡设备发起 URL 访问请求
- CDN 全局负载均衡设备根据用户 IP 地址，以及用户请求的 URL，选择一台用户所属区域的负载均衡设备，并将请求转发到此设备上
- 然后区域的负载均衡设备会选择一个最优的缓存服务器，然后得到其 IP 地址，将这个地址返回给全局负载均衡的设备
- 根据用户的 IP 地址，判断那个边缘节点距离用户最近
- 根据用户所请求的 URL 中携带的内容名称，判断那个边缘节点上有用户所需内容
- 查询各个边缘节点当前的负载情况，判断哪一个边缘节点尚有服务能力
- 全局负载均衡设备吧服务器的 IP 地址返回给用户
- 用户向缓存服务器发起请求，缓存服务器响应用户请求， 将用户所需内容传送到用户终端。如果这台缓存服务器上并没有用户想要的内容，而区域负载均衡设备依然将它分配给了用户，那么这台服务器就要向它的上一级缓存服务器请求内容，直至追溯到网站的源服务器将内容拉到本地。



CDN 的四大技术：


- 内容发布：将内容发布或投递到距离用户最近的远程服务点（POP）处
- 内容存储：对于 CDN 系统而言，需要考虑两个方面的内容存储问题，一个是内容源的存储，一个内容再 Cache 节点中存储
- 内容路由：它是整体性的网络负载均衡技术，通过内容路由器中的重定向机制，在多个远程 POP 上均衡用户的请求，以使用户得到最近的内容源的想要
- 内容管理



- [https://juejin.im/post/6844903906296725518](https://juejin.im/post/6844903906296725518)
- [https://mp.weixin.qq.com/s/eSpLgQPuvSfTKnwZLUlocQ](https://mp.weixin.qq.com/s/eSpLgQPuvSfTKnwZLUlocQ)



## 说一说平时如何做性能优化？


- SSR（服务器端渲染）
- 首屏加载时，SSR 为什么会比客户端渲染要快
- SSR 如何保证前后端拿到的数据 API 是一致的



主要遇到了什么问题？


> - 页面的 DOM 完全由 JS 来渲染，使得大部分搜索引擎无法爬取渲染后的真实 DOM，不利于 SEO。
> - 页面首屏内容到达时间强依赖于 JS 静态资源的加载（因为 DOM 的渲染由 JS 来执行）使得网络状况差的情况下，白屏时间大幅上升。



主要依赖什么技术？


> Node 的发展 + Virtual DOM 的前端框架的出现



主要解决了什么问题？


> - 服务端渲染的首屏直出，使得输出到浏览器的就是完备的html字符串模板，浏览器可以直接解析该字符串模版，因此首屏的内容不再依赖js的渲染。
> - 正是因为服务端渲染输出到浏览器的是完备的html字符串，使得搜索引擎能抓取到真实的内容，利于SEO。



如果我的需求只是生成文案类的静态页面，需要用到服务端渲染吗？


> A：像这些和用户状态无关的静态页面，完全可以采用预渲染的方式（具体见Vue SSR官方指南），服务端渲染适用的更多场景会是状态相关的（比如用户信息相关），需要经过CPU计算才能输出完备的html字符串，因此服务端渲染是一个CPU密集型的操作。而静态页面完全不需要涉及任何复杂计算，通过预渲染更快且更节省CPU资源。



为什么 CSR 会比 SSR 要慢？


> - 因为根据关键渲染路径，SSR 得到的就是一个生成好的 HTML 模板，不需要加载解析 JS 等内容，客户端获取之后，就可以直接开始渲染流程
> - CSR 需要发起请求获取 HTML/CSS/JS 等内容，然后 JS 来处理渲染，所以需要等待 JS 加载，而且 JS 可能很大，就容易造成关键字节大，这样要来回几次获取，才能开始渲染，所以整体上来说 CSR 渲染有问题



怎么保证前后端有一样的接口？（问题版）


- 双端路由如何维护？
   - 后端  `server` 通过 `/` 定义路由，但是 React SPA 模式下使用 `react-router` 来定义路由，是不是需要维护两套路由？
- 获取数据的方法和逻辑写在哪里？
   - 数据获取的 `fetch` 写得独立的方法，和组件没有任何关联，每个路由都要有自己独立的方法
- 服务器端 html 节点无法重用？
   - 即使服务器端得到数据，也渲染到浏览器内，但是当浏览器进行组件渲染的时候直出的内容会一闪而过消失



如何解决问题？（同构才是核心）


> 所谓同构就是采用一套代码，构建双端（server 和 client）逻辑，最大限度的重用代码，不用维护两套代码。React 出现得到了比较广泛的应用。React `renderToString` 渲染成 HTML 模板，使用 renderToNodeStream ，流失输出来提升服务端渲染性能。（内容直出）



- 路由：双端使用同一套路由规则，服务器端同构 `matchRoutes` 进行路由查找
- 数据同构（prefetch 同构）：给组件定义一个 static 方法，然后用于异步数据的获取，比如 `getInitialProps` Next.js 的
- 渲染同构：数据注水：同构 window.__INIT_STATE__是 `id=krs-server-render-data-BOX ` 来标志，然后数据脱水，同构 `Provider` 注入数据，然后客户端检测，渲染模板，对比模板，一样的话就不重新渲染，否则重新渲染，然后再 `componentDidMount` 里面进行事件的激活。



- [https://tech.youzan.com/server-side-render/](https://tech.youzan.com/server-side-render/)
- [https://ssr.vuejs.org/zh/](https://ssr.vuejs.org/zh/)
- [https://juejin.im/post/6844903943902855176](https://juejin.im/post/6844903943902855176)



## React 生命周期

- 为什么要创造新的生命周期了？
- 组件上面的 key 是干什么的？如何 key 发生了变化 React 会如何做？



### React 最新的生命周期说一下


![image.png](https://cdn.nlark.com/yuque/0/2020/png/123790/1592807254814-102ffd3d-863e-4ca0-9e20-f722014b3d03.png#align=left&display=inline&height=593&margin=%5Bobject%20Object%5D&name=image.png&originHeight=1186&originWidth=2192&size=424711&status=done&style=none&width=1096)


React 16 之前：


- 挂载：
   - constructor：初始化 state，绑定方法
   - componentWillMount：不能 setState，因为还没渲染
   - render：纯函数
   - componentDidMount：可以操作 DOM、发起网络请求、挂在定时器
- 更新：
   - componentWillReceiveProps：从父组件收到新属性
   - shouldComponentUpdate：是否更新
   - componentWillUpdate
   - render
   - componentDidUpdate
- 卸载
   - componentWillUnmount：清除定时器，删除无用的 DOM，



React 16 及之后：


- 挂载：
   - constructor
   - getDerviedStateFromProps
   - render
   - componentDidMount
- 更新
   - getDerivedStateFromProps
   - shouldComponentUpdate
   - render
   - getSnapshotBeforeUpdate
   - componentDidUpdate
- 卸载
   - componentWillUnmount



### 参考链接


- [https://juejin.im/post/5b6f1800f265da282d45a79a#heading-12](https://juejin.im/post/5b6f1800f265da282d45a79a#heading-12)



### 为什么要创造新的生命周期了？


原因是因为：


- 这些废弃的生命周期方法经常被误用或者滥用，并且在 V16.0 之后，React 更新了其渲染机制，通过异步的方式进行渲染，在 render 之前所有函数都可能被执行多次
   - 比如 `UNSAFE_componentWillMount`  里面发起 AJAX 请求
   - 废弃了 `componentWillUpdate` ，由 `getSnapshotBeforeUpdate` 取代，是在 DOM 组件真正更改前调用，获取到的信息更加可靠
   - 废弃 `componentWillReceiveProps` ，由 `getDerivedStateFromProps` 取代



所以使用新的 API，规范用法，防止出错，提高性能。

- [https://juejin.im/post/6844904199923187725](https://juejin.im/post/6844904199923187725)



## 单个并发限制异步
实现一个串行的后台接口加载函数, 要求上一个接口加载完毕，再加载下一个接口
```javascript
async function request(urls) {
  if (urls.length === 0) return;
  
  const url = urls.shift();
  await fetch(url);
  
  return request(urls);
}
```
## Limit 的并发异步限制
实现一个图片并行加载函数，要求仿照浏览器域名请求并发限制的策略，urls 为要加载的图片地址， limit 为同时请求并发的上限,只考虑所有请求都会成功的情况
```javascript
function loadImage(urls, limit) {
  const asyncLimitRequest = new AsyncLimitRequest(limit);
  
  urls.forEach(url => asyncLimitRequest.request(() => fetch(url)));
}

class AsyncLimitRequest {
  constructor(limit) {
    this.limit = limit;
    this.promiseList = [];
    this.curr = 0;
  }
  
  async request(promise) {
    this.curr++;
    
    if (this.curr > this.limit) {
      return this.promiseArr.push(promise);
    }
    
    await promise();
    
    this.curr--;
    
    const shiftPromise = this.promiseArr.shift();
    return shiftPromise;
  }
}
```
## CSS 的标签优先级



- !important 标签的
- 内联
- id
- class
- 标签
- 继承



## 说一说 HTTP 缓存？


- 强缓存（不需要发送 HTTP）
- 协商缓存（发送 HTTP）



浏览器中缓存分为两种，一种强缓存一种协商缓存。


### 强缓存


强缓存不需要发送 HTTP 请求，直接获取，具体通过以下几种情况来检查：


- Http/1.0 使用 Expires 字段记录服务器端的过期时间
- Http/1.1 使用 Cache-Control 的 max-age 字段，过期时长来检查
- 当然 Cache-Control 还可以设置几个其他的值
   - no-cache：直接跳过强缓存，进入协商缓存阶段
   - no-control：不使用任何缓存
   - private：不允许中间服务器缓存
   - public：浏览器和中间服务器都可以缓存
   - s-maxags：中间服务器缓存的过期时长



当 Expires 和 Cache-Control 同时存在时，Cache-Control 优先级更高。


### 协商缓存


发起 HTTP 请求，在请求头中携带 缓存 tag 来向服务器发生请求，服务器根据 tag 来觉得是否使用缓存，这个就是协商缓存。


这个 tag 有两种：


- Last-Modified，在请求头中加入 If-Modified-Since 具体记录了 Expires，如果请求头中的值小于服务器端相应的最后修改时间，那么就要更新内容，与具体的 HTTP 请求响应流程一致，否则返回 304，告诉浏览器直接用缓存
- Etag：是服务器根据当前文件的内容，生成 hash 标志，如果内容变，那么 hash 会变，服 务器通过响应头将这个标志给浏览器，下次请求将这个值作为 If-None-Match 这个字段内容作为请求体发送给服务器，服务器收到后，如果内容一致就 304，用缓存，不一致返回新的资源。



## 能说一说 GC（垃圾收集器）的原理嘛？




- 标记清除
- 引用计数



### 标记清除


当变量进入环境，就将变量标记为 “进入环境”，当变量离开环境，则将变量标志为 “离开环境”


- 垃圾收集器（GC）会给存储在内存中的变量打上标记
- 然后，去除运行环境中的变量以及被环境中所引用变量的标记
- 此后，依然有标记的变量被视为准备删除的变量
- 最后，垃圾收集器完成内存清除工作，销毁那些带标记的值然后回收它们占用的内存空间



### 引用计数


每被引用一次就 +1， 删除引用就 -1，引用次数为0就代表要被回收，会有循环引用问题


### 参考链接


- [https://juejin.im/post/5ad3f1156fb9a028b86e78be](https://juejin.im/post/5ad3f1156fb9a028b86e78be)
