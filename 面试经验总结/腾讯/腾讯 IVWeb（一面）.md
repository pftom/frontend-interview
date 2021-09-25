因为是线下面试，我在 11 楼，他在 9 楼，所以没有自我介绍环节：


- 一开始一堆的介绍，缓解我的心情，IVWeb 是干什么的，我为什么要选择 IVWeb？为什么一开始问 IMWeb ？
- DNS 是什么？
   - 如何查询？
- CDN 有了解嘛？
- 如何做性能优化？
   - 关键渲染路径做性能优化
   - 知道 SSR 嘛？服务器端渲染如何做？
   - 如何解决渲染白屏问题？
- 说一说 HTTP 缓存？
- HTTPS 是什么？
   - 如何保证安全
   - 说一下通信过程？
   - 如果伪造数字证书了怎么办？（找可信的 CA 机构签发）
- React 的源码了解过嘛？
   - React 特征是什么？
   - Virtual DOM 是什么？
   - Diff 算法如何运作？
      - 底层如何比较 Key，如何进行替换，如何进行移动？
- 7点15分，时针和分针之间的夹角是多少？（小于 180 度的）
- 对 Redux 了解嘛？
   - 它是干什么的？它的特点是什么？为什么要用它？
   - 为什么 React 需要它？
   - 还有那些一样的库？
   - 我们在 React 中还可以怎么替换 Redux，可以使用什么样的手段？
- HTTP 常用的状态码，分别是干什么的？
- 你遇到过最难的一件事情是什么？
- 你有什么想问我的？



## 什么是 DNS？


域名服务器（Domain Name System）


- 互联网协议（一）：自下而上，[https://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html](https://www.ruanyifeng.com/blog/2012/05/internet_protocol_suite_part_i.html)
- 互联网协议（二）：自上而下，[https://www.ruanyifeng.com/blog/2012/06/internet_protocol_suite_part_ii.html](https://www.ruanyifeng.com/blog/2012/06/internet_protocol_suite_part_ii.html)
- [https://zhuanlan.zhihu.com/p/25563675](https://zhuanlan.zhihu.com/p/25563675)
- [https://www.ruanyifeng.com/blog/2016/06/dns.html](https://www.ruanyifeng.com/blog/2016/06/dns.html)



## CDN 有了解嘛？


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
- 内容管理：它通过内部和外部监控系统，获取网络部件的状况信息，测量内容发布的端到端性能（如包丢失、延时、平均带宽、启动时间、帧速率等），保证网络处于最佳的运行状态。



- [https://juejin.im/post/6844903906296725518](https://juejin.im/post/6844903906296725518)
- [https://mp.weixin.qq.com/s/eSpLgQPuvSfTKnwZLUlocQ](https://mp.weixin.qq.com/s/eSpLgQPuvSfTKnwZLUlocQ)



## 如何做性能优化？


> [《Designing for Performance》](http://shop.oreilly.com/product/0636920033578.do)的作者 [Lara Swanson](http://radar.oreilly.com/lswanson) 在2014年写过一篇文章[《Web性能即用户体验》](https://www.forbes.com/forbes/welcome/?toURL=https://www.forbes.com/sites/oreillymedia/2014/01/16/web-performance-is-user-experience/&refURL=&referrer=#194909aa5a52)，她在文中提到“网站页面的快速加载，能够建立用户对网站的信任，增加回访率，大部分的用户其实都期待页面能够在2秒内加载完成，而当超过3秒以后，[就会有接近40%的用户离开你的网站](http://www.mcrinc.com/Documents/Newsletters/201110_why_web_performance_matters.pdf)”。



### 什么是关键渲染路径？
我记得，有一个非常经典的面试题叫做：《当浏览器地址栏输入URL并回车后，发生了什么？》。


关键渲染路径就是描述浏览器从收到 HTML、CSS 和 JavaScript 字节开始，到如何使用HTML、CSS 和 JavaScript 在屏幕上渲染像素的中间过程。


如果我们能够优化这条路径，就能让页面更快速的展示内容，给用户更好的体验。


步骤：


1. 处理 HTML 标记并构建 DOM 树
1. 处理 CSS 标记并构建 CSSDOM 树
1. 将 DOM 与 CSSDOM 合并成一个渲染树
1. 根据渲染树来布局。
1. 将各个节点绘制到屏幕上



如果 DOM/CSS 变化，会再次执行这个步骤。


- 什么是关键资源：可能阻止网页首次渲染的资源
- 最短关键路径长度：获取所有关键资源所需要的往返次数或总时间
- 关键字节：实现网页首次渲染所需的总字节数，它是所有关键资源传送文件大小的总和。我们包含单个 HTML 页面的第一个示例包含一项关键资源（HTML 文档）；关键路径长度也与 1 次网络往返相等（假设文件较小），而总关键字节数正好是 HTML 文档本身的传送大小。





HTML、CSS 和 JavaScript 都是关键渲染资源：


为尽快完成首次渲染，我们需要最大限度减小以下三种可变因素：


- 关键资源的数量。
- 关键路径长度。
- 关键字节的数量。



关键资源是可能阻止网页首次渲染的资源。这些资源越少，浏览器的工作量就越小，对 CPU 以及其他资源的占用也就越少。


同样，关键路径长度受所有关键资源与其字节大小之间依赖关系图的影响：某些资源只能在上一资源处理完毕之后才能开始下载，并且资源越大，下载所需的往返次数就越多。


最后，浏览器需要下载的关键字节越少，处理内容并让其出现在屏幕上的速度就越快。要减少字节数，我们可以减少资源数（将它们删除或设为非关键资源），此外还要压缩和优化各项资源，确保最大限度减小传送大小。


**优化关键渲染路径的常规步骤如下：**

1. 对关键路径进行分析和特性描述：资源数、字节数、长度。
1. 最大限度减少关键资源的数量：删除它们，延迟它们的下载，将它们标记为异步等。
1. 优化关键字节数以缩短下载时间（往返次数）。
1. 优化其余关键资源的加载顺序：您需要尽早下载所有关键资产，以缩短关键路径长度。



- ThoughtWorks 前端不止：Web 性能优化 - 关键渲染路径以及优化策略：[https://insights.thoughtworks.cn/critical-rendering-path-and-optimization-strategy/](https://insights.thoughtworks.cn/critical-rendering-path-and-optimization-strategy/)
- 通过 DevTools 观察对应的状况并进行处理：[https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp?hl=zh-cn](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/analyzing-crp?hl=zh-cn)
- Navigation Timeing API: [https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp?hl=zh-cn](https://developers.google.com/web/fundamentals/performance/critical-rendering-path/measure-crp?hl=zh-cn)
- [https://github.com/berwin/Blog/issues/29](https://github.com/berwin/Blog/issues/29)
- [https://zhuanlan.zhihu.com/p/57752042](https://zhuanlan.zhihu.com/p/57752042)



### 知道 SSR 嘛？服务器端渲染如何做？


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
   - 后端  `server` 通过 `/` 定义路由，但是 React SPA 模式下使用 `react-router` 来定义路由，是不是需要维护两套路由？
- 获取数据的方法和逻辑写在哪里？
   - 数据获取的 `fetch` 写得独立的方法，和组件没有任何关联，每个路由都要有自己独立的方法
- 服务器端 html 节点无法重用？
   - 即使服务器端得到数据，也渲染到浏览器内，但是当浏览器进行组件渲染的时候直出的内容会一闪而过消失



如何解决问题？（同构才是核心）


> 所谓同构就是采用一套代码，构建双端（server 和 client）逻辑，最大限度的重用代码，不用维护两套代码。React 出现得到了比较广泛的应用。React `renderToString` 渲染成 HTML 模板，使用 renderToNodeStream ，流失输出来提升服务端渲染性能。（内容直出）



- 路由：双端使用同一套路由规则，服务器端同构 `matchRoutes` 进行路由查找
- 数据同构（prefetch 同构）：给组件定义一个 static 方法，然后用于异步数据的获取，比如 `getInitialProps` Next.js 的
- 渲染同构：数据注水：同构 window.__INIT_STATE__是 `id=krs-server-render-data-BOX ` 来标志，然后数据脱水，同构 `Provider` 注入数据，然后客户端检测，渲染模板，对比模板，一样的话就不重新渲染，否则重新渲染，然后再 `componentDidMount` 里面进行事件的激活。



- [https://tech.youzan.com/server-side-render/](https://tech.youzan.com/server-side-render/)
- [https://ssr.vuejs.org/zh/](https://ssr.vuejs.org/zh/)
- [https://juejin.im/post/6844903943902855176](https://juejin.im/post/6844903943902855176)



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



## HTTPS 是什么？


### 如何保证安全？


HTTPS 是在 HTTP 和 TCP 之间建立了一个安全层，HTTP 与 TCP 通信的时候，必须先进过一个安全层，对数据包进行加密，然后将加密后的数据包传送给 TCP，相应的 TCP 必须将数据包解密，才能传给上面的 HTTP。


### 说一下通信过程


浏览器传输一个 client_random 和加密方法列表，服务器收到后，传给浏览器一个 server_random、加密方法列表和数字证书（包含了公钥），然后浏览器对数字证书进行合法验证，如果验证通过，则生成一个 pre_random，然后用公钥加密传给服务器，服务器用 client_random、server_random 和 pre_random ，使用公钥加密生成 secret，然后之后的传输使用这个 secret 作为秘钥来进行数据的加解密


属于 OSI 七层模型的会话层。TLS 1.0 被标准化 = SSL 3.1


### 如果伪造了数字证书怎么办？


找可信的 CA 机构进行签发


## React 的源码了解过嘛？


### React 特征是什么？


Lifting-State-Up、单向数据量、函数式编程风格、pull-based 系统（数据发生变化需要 setState 手动去通知系统更新，Push-based Vue 响应式系统自己会更新）


### Virtual DOM 是什么？


对真实 DOM 的 JS 对象映射，至少包含三个值：标签名（tag），属性（props），子元素对象（children）。


优点：


- VD 最大的特点是将页面的状态抽象为 JS 对象的形式，配合不同的渲染工具，使得跨平台成为可能，如 React 借助 VD 实现了服务器端渲染、浏览器渲染和移动端渲染等功能
- 在进行页面更新的时候，借助 VD，DOM 元素的改变可以在内存中进行比较，然后借助框架的事务机制，将多次改变结果合并后一次更新到页面，从而有效的减少页面的渲染次数，提高渲染效率。



使用 VD：UI = f(state)   UI = render(state)


- 使用 JS 的计算时间换渲染时间



### Diff 算法如何运作？


传统的 diff 算法的复杂度为 O(n^3)，而 React 制定大胆的策略，将 O(n^3) 复杂度的问题转换为 O(n) 复杂度的问题。


1. Tree Diff：Web UI 中 DOM 节点跨层级移动操作特别是，可以忽略不计
   1. 只会对同一父节点下的所有子节点比较，如果子节点不存在，就删除，不会进一步比较。这样只需要一次遍历，就能完成整个 DOM 树的比较。
   1. 建议：在开发组件时，保持稳定的 DOM 结构有助于性能提升
2. Component Diff：两个相同组件产生类似的 DOM 结构，不同的组件产生不同的 DOM 结构；
   1. 同一类型组件，按原策略继续进行 Virtual DOM 比较
   1. 如果不是, 则判断此组件为 dirty component,则会替换该组件下的所有节点
   1. 对于同一类型的组件,有可能其 Virtual DOM 没有变化，如果能够确切的知道这一点可以节省大量的 Diff 运行时间，因此 React 通过在组件内允许用户通过 shouldComponentUpdate 来判断是否需要 Diff，也可以直接继承 PureComponent，然后自动帮你完成
3. Element Diff：对于同一层次的一组子节点，它们可以通过唯一的 id 进行区分。所以只需要给同一组的元素给与不同的 key 值就可以了
   1. 提供三种操作：INSERT_MARKUP、MOVING_EXISTING、REMOVE_NODE
   1. 添加唯一的 Key，Key 变化就进行插入和删除，否则移动
   1. 在开发过程中，尽量减少将最后一个节点移动到列表首部的操作，当节点的数量过大或者更新过于频繁时，在一定程度上会影响 React 的渲染性能。



因此通过上面的三个策略，可以让用户无需顾忌性能问题而 “任性自由” 的刷新界面，同时也无需关系 Virtual DOM 背后运作的实际原理，因为 React Diff 会帮助我们计算出实际变化的 Virtual DOM 部分，然后进行实际的 DOM 操作 。所以说 React Diff 和 Virtual DOM 是保证 React 性能口碑的幕后推手。


#### 底层如何比较 key，如何进行替换，如何进行移动？


查看这个链接：[https://zhuanlan.zhihu.com/p/20346379](https://zhuanlan.zhihu.com/p/20346379)，讲得很清楚


## 对 Redux 了解嘛？


- 单向数据流
- 基于中间件



一个单向数据流的状态管理库，可用于 React/Vue/Angular 等框架。


状态类型：


- Domain State：服务器端的状态，与数据库保持同步
- UI State：交互有关的状态，模态框的开关、页面的 loading、选中状态等

![5e7043970001ac7708770462.png](https://cdn.nlark.com/yuque/0/2020/png/123790/1592904371576-029206f3-18ef-45cc-8108-a236b9f5147c.png#align=left&display=inline&height=462&margin=%5Bobject%20Object%5D&name=5e7043970001ac7708770462.png&originHeight=462&originWidth=877&size=41862&status=done&style=none&width=877)
### 原理


Redux 是 JavaScript 状态容器，提供可预测化的状态管理。让每个 State 变化都可预测，将应用中所有的动作与状态统一管理，让一切有迹可循。


#### 发布-订阅模式


> View 中通过 Connect 订阅 Store 的修改
> 一旦 Store 修改,就会通知所有的订阅者, View 接收到通知之后会使用新的状态重新渲染

### 
它任务：


- Web 应用是一个状态机，试图与状态一一对应
- 所有的状态，保存在一个全局的状态对象里面



#### 三大原则


- 单一数据源，一个应用一般只有一个 Store
- State 是只读的，唯一改变 State 的方式是 dispatch 一个 action，action 描述了修改状态的相关信息。便于调试和进行重做（也就是 time traveling）
- 通过纯函数来修改，需要编写 Reducer 来接收  Action 进行实际状态的修改，Reducer 接收上一次的 State 和 Action，返回新的 State。只要传入的 Action 相同，那么返回的新状态一定是一样的结果。



### 流程


- 用户触发页面上的操作，dispatch 发送一个 action
- Redux 接收到这个 Action 后通过 Reducer 函数计算出下一个状态
- 将新的状态更新进 Store，Store 更新之后通知页面进行重新渲染



### dispatch 原理


- 想要修改 Store 状态的唯一方法，就是 dispatch 一个 action
- dispatch 主要就是通知 Store 调用 Reducers 函数，接收对于的 action，然后进行状态的改变，返回下一个状态。
- dispatch 的返回值时它 dispatch 的 action 本身，这使得 Redux 基于 dispatch 引入了一个 middlewares 中间件的概念，可以通过一系列中间件去增强 dispatch 的功能，比如 redux-logger，就是在 dispatch 修改状态的前后打印对于的 dispatch 方法、前后状态，方便调试
- 中间件的执行是一个洋葱圈，即从最外层的前半部分一个个执行，执行到最内层函数，然后从最内层函数的后半部分执行，执行到最外层的后半部分，这种执行逻辑类似一个洋葱圈，同样应用洋葱圈模型的还有 koa、vuex 等
- 甚至可以通过异步中间件，来增强 dispatch 来处理异步流程



### 还有那些一样的库？


Recoil、Hox、Mobx 等


### 我们在 React 中还可以怎么替换 Redux， 可以使用什么样的手段？


- 可以用 Context API
- 可以使用事件的发布订阅机制
- 全局变量 
   - 缺点是：
      - 无法模块化
      - 状态混乱，无法追踪





## HTTP 常用的状态码，分别是干什么的？


- 1xx：表示目前是协议的中间状态，还需要后续请求
- 2xx：表示请求成功
- 3xx：表示重定向状态，需要重新请求
- 4xx：表示请求报文错误
- 5xx：服务器端错误



常用状态码：


- 101 切换请求协议，从 HTTP 切换到 WebSocket
- 200 请求成功，有响应体
- 301 永久重定向：会缓存
- 302 临时重定向：不会缓存
- 304 协商缓存命中
- 403 服务器禁止访问
- 404 资源未找到
- 400 请求错误
- 500 服务器端错误
- 503 服务器繁忙





