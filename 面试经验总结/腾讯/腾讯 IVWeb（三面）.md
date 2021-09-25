- 前端监控有了解吗？
- CDN 是什么？
- 如何解决白屏问题？
- 说一说你最有挑战的一个项目？
- PWA 有了解吗？
- SSR 有了解吗？
- 页面卡住了可能是因为什么？
- TypeScript 知道吗？
- 说一说 XSS 和 CSRF
- 说一说 JS 的 Event Loop
- 移动端跨平台开发的各种技术



## 前端监控有了解吗？



- [雅虎性能军规（34 条）](https://www.cnblogs.com/li0803/archive/2009/09/20/1570581.html)
- 页面性能主要转化为如下三个方面：
   - 响应速度：页面访问速度 + 交互响应速度（100 毫秒）
      - 页面访问速度：白屏、FP、FCP、FMP、Time to Interactive（TTI）、长任务（最大化空闲时间）
      - performance.getEntriesByType('paint')
   - 页面稳定性：页面出错率低
      - 资源加载出错
         - window.addEventListener('error')
      - JS 执行出错
         - window.onerror
         - promise reject / unhandledrejection
         - window.console.error
         - window.frames[0].onerror
   - 外部服务调用：网络请求访问速度
      - CGI 耗时
      - CGI 成功率
         - window.XMLHttpRequest 和 window.fetch 捕获错误
      - CDN 资源耗时
- 主要监控的 API：
   - PerformanceObserver API
   - window.performance.navigation：页面是加载还是刷新、发生了多少次重定向
   - Resource Timing  API
   - Paint Timing API
   - User Timing API
   - High Resolution Time  API
   - Performance Timeline API
   - window.performance.timing：页面加载的各阶段时长
   - window.performance.memory：基本内存使用情况，Chrome 添加的一个非标准扩展
   - performance.getEntries/getEntriesByName/getEntriesByType/now 等



四个方面：


- 前端监控的两种主要技术方案
- 怎么样去做一个真实用户性能数据的采集
- 怎么去分析这些数据
- 这些数据如何运用



### 前端性能监控分为两种方式：


- 合成监控（SYM）
- 真实用户监控（RUM）



#### 合成监控


Lighthouse：


- 性能相关数据
- PWA
- SEO
- 前端工程化的最佳实践



#### 真实用户监控


- 真实用户访问
- 提取性能指标
- 数据清洗加工
- 性能分析监控

![5c6ba77637c91.png](https://cdn.nlark.com/yuque/0/2020/png/123790/1598702753888-53045015-8615-4153-b603-afd94525ecb4.png#align=left&display=inline&height=502&margin=%5Bobject%20Object%5D&name=5c6ba77637c91.png&originHeight=502&originWidth=2176&size=151127&status=done&style=none&width=2176)


![5c6ba808c121c.png](https://cdn.nlark.com/yuque/0/2020/png/123790/1598692429777-c6177dad-f1b7-43c9-aaf4-4132be8252e7.png#align=left&display=inline&height=936&margin=%5Bobject%20Object%5D&name=5c6ba808c121c.png&originHeight=936&originWidth=1932&size=237304&status=done&style=none&width=1932)
#### 
### 真实用户性能数据采集方案


- 使用标准的 API
- 定义合适的指标
- 采集正确的数据
- 上报关联的维度



#### 使用标准的 API


High Resolution Time（HRTime）


- 高精度（纳秒级别）
- 单调递增（不受系统时间影响）



最佳实践：


- 采集性能数据时先抹平 Navigation Timeing Spec 的差异
- 优先使用 Performance Timeline API（复杂场景考虑 PerformanceObserver）



#### 定义合适的指标


- 页面加载时长（Dom load、Dom complete）：无法准确反映加载性能，易受
- 首次渲染和首次内容渲染（FP、FCP）：聚焦于页面元素的渲染，相对而言更客观，但是像素出来了，不一定是用户关心的内容
- 首次有效渲染（FMP）：当一个网页的 DOM 结构发生剧烈变化的时候，就是这个网页内容出现的时候，那么在这样一个时间点上，就是用户看到主要内容的一个时间点
- 开始渲染时间（Start Render Time，SRT）：很难通过 JS 在用户环境获取，就是类似拿着个照相机不断的对着屏幕拍，直到出现第一个不一样，那就是开始渲染时间。



![5c6bbb8aa3dec.png](https://cdn.nlark.com/yuque/0/2020/png/123790/1598694488381-e1b5aba3-b006-4c8b-bed2-495624be9a65.png#align=left&display=inline&height=572&margin=%5Bobject%20Object%5D&name=5c6bbb8aa3dec.png&originHeight=572&originWidth=1546&size=192113&status=done&style=none&width=1546)


最佳实践：


- 根据业务特性及性能监控方案选择最合适的性能指标
- 必要情况下可以自定义性能点位



#### 怎么样采集正确的数据？


![5c6bce24ba464.png](https://cdn.nlark.com/yuque/0/2020/png/123790/1598703412372-eb5279c5-a197-47c2-9fd0-10e60156a6e6.png#align=left&display=inline&height=578&margin=%5Bobject%20Object%5D&name=5c6bce24ba464.png&originHeight=578&originWidth=1662&size=214316&status=done&style=none&width=1662)
这是目前最标准的一个 RUM 模型，一个页面的加载状况被定义为很多阶段。


但是每个阶段不一定会发生，因为：


- 同域名 TCP 链接并发限制
- 浏览器正在分配缓存空间
- 更高优先级请求正在处理



这会导致，页面存在一个 stalled 时间，但这个时间不会被记入规范的 RUM 模型，而且这个很常见；还有就是浏览器会对 DNS 查询做缓存，可能这次需要时间，下次就不需要时间了。


所以不能在计算阶段再上报数据，而是上报页面加载开始时间，以及后续各时间点相对增量，在数据段进行阶段清洗和异常处理。比如页面开始加载时间为 0 毫秒，TCP 开始时间为 2 毫秒，相对开始的增量。


#### 上报关联的维度


做前端数据采集的时候，维度数据非常重要，主要包含两个维度：1）常规维度 2）专业维度


- 常规维度：
   - 时间
   - 页面
   - 浏览器
   - 操作系统
   - 地理位置
- 专业维度
   - 页面是否可见
   - 页面加载方式
   - 等效网络连接类型
   - 是否启用 Service Worker
   - 是否启用 HTTP/2 或者 SPDY
   - 是否启用了 GZip 压缩等



这些数据都要尽可能的采集到，从而能够更好的去分析我们的页面性能。


### 准确的分析性能数据及影响因素


- 页面加载时长会收到很多异常因素影响。
   - 比如用户已经在正常使用你的网站了，但是有一个小 icon 没加载出来，就会导致这个指标超长
- 存在很多长尾数据
   - 比如 JS 太大了
   - 静态资源太多，没有压缩
   - 用户网络状况不好
   - 服务器压力突然很高



还有存在影响页面加载性能的因素：


- 比如 Chrome 68 的全新浏览器的架构模式，对不可见的 Tab 的硬件资源分配做了一个极大的限制
- 浏览器替我们做了很多缓存，比如静态资源、304 没有更新、或者前进后退秒开，没有出现完整刷新的效果
- Service Worker 可以做强大的缓存，可以影响整个页面的性能



#### 最佳实践


在分析性能指标的时候，建议关注百分位数，对性能要求越高，使用越大的百分位数。


- 比如 95 分位
- 平均数
- 10% 截尾平均数



比如承诺 99% 的用户都要小于 5 秒，那么看页面加载时常就应该看 99 分位数，如果经历不够，只能承诺 50% 的人页面加载时长小于 5 秒，实际上 50 分位数，就是中位数，就是 50% 的访问能够不小于这个时间打开这个页面。


### 数据如何运用


- 自己的数据+性能数据横向对比+美观度+用户感知度+任务转化率 = 体验分，给用户一个分析的抓手，告诉他性能的瓶颈，综合情况是怎么样的，让用户感知他的业务到底是快还是慢
- 使用截尾平均值，保持数据的平稳，异常值不会产生太大的影响，关心自己相比之前是快还是慢



#### 参考链接


- [https://www.infoq.cn/article/Dxa8aM44oz*Lukk5Ufhy](https://www.infoq.cn/article/Dxa8aM44oz*Lukk5Ufhy)
- [http://www.alloyteam.com/2020/01/14184/#prettyPhoto](http://www.alloyteam.com/2020/01/14184/#prettyPhoto)
- [http://fex.baidu.com/blog/2014/05/build-performance-monitor-in-7-days/](http://fex.baidu.com/blog/2014/05/build-performance-monitor-in-7-days/)
- [http://fex.baidu.com/blog/2014/05/front_end-data/](http://fex.baidu.com/blog/2014/05/front_end-data/)



## CDN 是什么？
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



## 如何解决白屏问题？页面卡住了可能是因为什么？


[从关键渲染路径的三个方面来说：](https://www.yuque.com/xmh/avfn8z/wrzl64#6wNgB)


- 关键渲染资源
- 关键渲染路径长度
- 关键字节



[同时也可以使用 SSR，服务器端渲染来说：](https://www.yuque.com/xmh/avfn8z/plst1e#U2rb0)


还可以从浏览器侧的一些方面考虑：


- 比如浏览器单域名、单网页最多支持 6 个 TCP 并发请求，多的需要等待空闲让进，然后就增加了关键渲染路径长度所占用的总时间
- 还有可能是单线程的 JS，在执行 CPU 密集型的操作，比如大规模的 Canvas 动画，那么可以使用 CSS3 的 will-change: transform; 使用合成线程去处理，可以调用底层的 GPU 资源进行加速处理，而且独立于浏览器进程，不会卡顿
- 或者热区太小了，没有点到，以为卡顿
- 使用 CDN、使用 Service Worker 进行缓存
- 使用 HTTP/2 来处理请求，多路复用，头部压缩
- 比如存在 JS 的错误，及时捕获错误，或者内存泄露，及时检索内存泄露。



### 参考资料


- [https://www.infoq.cn/article/C77Hz2UzOepEIdtbvhK3?utm_source=related_read_bottom&utm_medium=article](https://www.infoq.cn/article/C77Hz2UzOepEIdtbvhK3?utm_source=related_read_bottom&utm_medium=article)
## PWA 有了解吗？


渐进式网络应用，是应用多项技术来改善用户体验的 Web App，它兼具 WebApp 和 Native App 的优点，核心技术包括：


- App Manifest
   - 主要是一个 manifest.json 文件
   - 可以添加应用的图标、名称、启动方式，主题颜色，访问路径等
   - 然后可以被添加到桌面，具有接近 Native App 的体验、启动动画，沉浸式体验
   - 可以被应用商店收录
- Service Worker
   - 独立于浏览器运行的脚本
   - 可以拦截网络请求、访问本地的缓存、接收服务器的离线推送
- Web Push（离线通知)、Notification API（展现）
- App Shell 和骨架屏
   - App Shell 是 PWA 展示所需资源的最小合集，每个页面的加载都需要这一部分资源，可以利用 Service Worker 把这个缓存在本地，填充 App 的框架，提升首屏速度和减少站点的流量
   - 而骨架屏就用于填充开始的内容，提升用户感知



用户体验是 PWA 的核心。


PWA 的主要特点是：


- 可靠：即使在网络不稳定的甚至断网的环境下，也能瞬间加载并实现
- 用户体验：快速响应，具有平滑的过渡动画及用户操作的反馈
- 用户黏性：和 Native App 一样，可以被添加到桌面，能接受离线通知，具有沉浸式的用户体验



PWA 强调渐进式：


- PWA 还在不断的进化，各种 API 等标准每年都会有不小的进步
- 标准设计的向下兼容，并且侵入性小，开发者使用新特性代价小，只需要在原有站点上新增，让站点的用户体验渐进式增强。



下面列出了 PWA 的最低要求：


- 需要 HTTPS
- 需要响应式
- 每个 URL 在断网的情况下都要有内容展现
- 支持 Web App Manifest，能添加到桌面
- 即使在 3G 网络下，页面加载也要快，可交互时间短
- 在主流浏览器下能正常展现
- 动画要流程，有用户操作反馈
- 每个页面都要有独立的 URL



### 参考链接


- [https://huangxuan.me/2017/07/12/upgrading-eleme-to-pwa/](https://huangxuan.me/2017/07/12/upgrading-eleme-to-pwa/)
- [https://lavas-project.github.io/pwa-book/thanks.html](https://lavas-project.github.io/pwa-book/thanks.html)



### 什么是 Service Worker？


Service Worker 是一种调度机制，可以适应 navigator.serviceWorker.register 来进行注册


- 是一个特殊的 worker 线程，独立于当前网页主线程，有自己的执行上下文
- 一旦被安装，就永远存在，除非显示取消注册
- 使用到的时候浏览器会自动唤醒，不用的时候自动休眠
- 可拦截并代理请求和处理返回，可以操作本地缓存，如 CacheStorage，IndexedDB
- 离线内容开发者可控
- 能接受服务器推送的离线消息
- 异步实现，内部借款异步化基本通过 Promise 实现
- 不能直接操作 DOM
- 必须在 HTTPS 下才能工作



## [SSR 有了解吗？](https://www.yuque.com/xmh/avfn8z/wrzl64#saHWa)


SSR 用起来主要的问题是什么？
## [TypeScript 知道吗？](https://www.yuque.com/xmh/avfn8z/uffl73#jGzaM)


### TypeScript 的泛型，手写一个  Pick 泛型工具函数


```typescript
// 这里使用了泛型约束、索引查询操作符和索引访问操作符
function pick<T, K extends keyof T>(obj: T, keys: K[]): T[k][] {
  
}

// 如果还需要返回一个对象的话，还需要使用到映射类型
function pick<T, K extends keyof T>(obj: T, keys: K[]): { [E in keyof T]?: T[E] } {
  
}
```


#### 参考链接


- [https://juejin.im/book/5da08714518825520e6bb810/section/5da08829f265da5ba273c1d0](https://juejin.im/book/5da08714518825520e6bb810/section/5da08829f265da5ba273c1d0)
## [说一说 XSS 和 CSRF](https://www.yuque.com/xmh/avfn8z/gidpks#jbt11)
## [说一说 JS 的 Event Loop](https://www.yuque.com/xmh/avfn8z/uffl73#5Enod)


## 移动端跨平台开发的各种技术


- [http://fex.baidu.com/blog/2015/05/cross-mobile/](http://fex.baidu.com/blog/2015/05/cross-mobile/)





