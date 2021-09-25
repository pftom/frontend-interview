---
# 主题列表：juejin, github, smartblue, cyanosis, channing-cyan, fancy, hydrogen, condensed-night-purple, greenwillow, v-green, vue-pro, healer-readable, mk-cute, jzman, geek-black, awesome-green, qklhk-chocolate
# 贡献主题：https://github.com/xitu/juejin-markdown-themes
theme: juejin
highlight:
---

> 注意：每道题前面出现的 (xx) 数字代表这道题出现的频次，此 JS 基础是基于 30+ 篇前端面经整理出的问题和对应的回答、参考链接等。文章内容为拿到 Offer 的本人整理。

## （2）问：0.1 + 0.2 === 0.3 嘛？为什么？

JavaScript 使用 Number 类型来表示数字（整数或浮点数），遵循 IEEE 754 标准，通过 64 位来表示一个数字（1 + 11 + 52）

- 1 符号位，0 表示正数，1 表示负数 s
- 11 指数位（e）
- 52 尾数，小数部分（即有效数字）

最大安全数字：Number.MAX_SAFE_INTEGER = Math.pow(2, 53) - 1，转换成整数就是 16 位，所以 0.1 === 0.1，是因为通过 toPrecision(16) 去有效位之后，两者是相等的。

在两数相加时，会先转换成二进制，0.1 和 0.2 转换成二进制的时候尾数会发生无限循环，然后进行对阶运算，JS 引擎对二进制进行截断，所以造成精度丢失。

所以总结：**精度丢失可能出现在进制转换和对阶运算中**

### 参考链接

- [https://juejin.im/post/5b90e00e6fb9a05cf9080dff](https://juejin.im/post/5b90e00e6fb9a05cf9080dff)

## （4）问：JS 数据类型

基本类型：Number、Boolean、String、null、undefined、symbol（ES6 新增的），BigInt（ES2020）
引用类型：Object，对象子类型（Array，Function）

### 参考链接

- [https://juejin.im/post/5b2b0a6051882574de4f3d96](https://juejin.im/post/5b2b0a6051882574de4f3d96)
- [https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Data_structures)

## 问：JS 整数是怎么表示的？

- 通过 Number 类型来表示，遵循 IEEE754 标准，通过 64 位来表示一个数字，（1 + 11 + 52），最大安全数字是 Math.pow(2, 53) - 1，对于 16 位十进制。（符号位 + 指数位 + 小数部分有效位）

## 问：Number() 的存储空间是多大？如果后台发送了一个超过最大自己的数字怎么办

Math.pow(2, 53) ，53 为有效数字，会发生截断，等于 JS 能支持的最大数字。

## （4）写代码：实现函数能够深度克隆基本类型

浅克隆：

```javascript
function shallowClone(obj) {
  let cloneObj = {};

  for (let i in obj) {
    cloneObj[i] = obj[i];
  }

  return cloneObj;
}
```

深克隆：

- 考虑基础类型
- 引用类型
  - RegExp、Date、函数 不是 JSON 安全的
  - 会丢失 constructor，所有的构造函数都指向 Object
  - 破解循环引用

```javascript
function deepCopy(obj) {
  if (typeof obj === "object") {
    var result = obj.constructor === Array ? [] : {};

    for (var i in obj) {
      result[i] = typeof obj[i] === "object" ? deepCopy(obj[i]) : obj[i];
    }
  } else {
    var result = obj;
  }

  return result;
}
```

## 问：事件流

事件流是网页元素接收事件的顺序，"DOM2 级事件"规定的事件流包括三个阶段：事件捕获阶段、处于目标阶段、事件冒泡阶段。
首先发生的事件捕获，为截获事件提供机会。然后是实际的目标接受事件。最后一个阶段是时间冒泡阶段，可以在这个阶段对事件做出响应。
虽然捕获阶段在规范中规定不允许响应事件，但是实际上还是会执行，所以有两次机会获取到目标对象。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>事件冒泡</title>
  </head>
  <body>
    <div>
      <p id="parEle">我是父元素 <span id="sonEle">我是子元素</span></p>
    </div>
  </body>
</html>
<script type="text/javascript">
  var sonEle = document.getElementById("sonEle");
  var parEle = document.getElementById("parEle");

  parEle.addEventListener(
    "click",
    function () {
      alert("父级 冒泡");
    },
    false
  );
  parEle.addEventListener(
    "click",
    function () {
      alert("父级 捕获");
    },
    true
  );

  sonEle.addEventListener(
    "click",
    function () {
      alert("子级冒泡");
    },
    false
  );
  sonEle.addEventListener(
    "click",
    function () {
      alert("子级捕获");
    },
    true
  );
</script>
```

当容器元素及嵌套元素，即在`捕获阶段`又在`冒泡阶段`调用事件处理程序时：**事件按 DOM 事件流的顺序**执行事件处理程序：

- 父级捕获
- 子级捕获
- 子级冒泡
- 父级冒泡

且当事件处于目标阶段时，事件调用顺序决定于绑定事件的**书写顺序**，按上面的例子为，先调用冒泡阶段的事件处理程序，再调用捕获阶段的事件处理程序。依次 alert 出“子集冒泡”，“子集捕获”。

### IE 兼容

- attchEvent('on' + type, handler)
- detachEvent('on' + type, handler)

### 参考链接

- [https://juejin.im/entry/5826ba9d0ce4630056f85e07](https://juejin.im/entry/5826ba9d0ce4630056f85e07)

## 问：事件是如何实现的？

基于发布订阅模式，就是在浏览器加载的时候会读取事件相关的代码，但是只有实际等到具体的事件触发的时候才会执行。

比如点击按钮，这是个事件（Event），而负责处理事件的代码段通常被称为事件处理程序（Event Handler），也就是「启动对话框的显示」这个动作。

在 Web 端，我们常见的就是 DOM 事件：

- DOM0 级事件，直接在 html 元素上绑定 on-event，比如 onclick，取消的话，dom.onclick = null，同一个事件只能有一个处理程序，后面的会覆盖前面的。
- DOM2 级事件，通过 addEventListener 注册事件，通过 removeEventListener 来删除事件，一个事件可以有多个事件处理程序，按顺序执行，捕获事件和冒泡事件
- DOM3 级事件，增加了事件类型，比如 UI 事件，焦点事件，鼠标事件

### 参考链接

- [https://zhuanlan.zhihu.com/p/73091706](https://zhuanlan.zhihu.com/p/73091706)

## 问：new 一个函数发生了什么

构造调用：

- 创造一个全新的对象
- 这个对象会被执行 [[Prototype]] 连接，将这个新对象的 [[Prototype]] 链接到这个构造函数.prototype 所指向的对象
- 这个新对象会绑定到函数调用的 this
- 如果函数没有返回其他对象，那么 new 表达式中的函数调用会自动返回这个新对象

## 问：new 一个构造函数，如果函数返回 `return {}` 、 `return null` ， `return 1` ， `return true` 会发生什么情况？

如果函数返回一个对象，那么 new 这个函数调用返回这个函数的返回对象，否则返回 new 创建的新对象

## 问：`symbol`  有什么用处

可以用来表示一个独一无二的变量防止命名冲突。但是面试官问还有吗？我没想出其他的用处就直接答我不知道了，还可以利用  `symbol`  不会被常规的方法（除了  `Object.getOwnPropertySymbols`  外）遍历到，所以可以用来模拟私有变量。

主要用来提供遍历接口，布置了  `symbol.iterator`  的对象才可以使用  `for···of`  循环，可以统一处理数据结构。调用之后回返回一个遍历器对象，包含有一个 next 方法，使用 next 方法后有两个返回值 value 和 done 分别表示函数当前执行位置的值和是否遍历完毕。

Symbol.for() 可以在全局访问 symbol

## （3）问：闭包是什么？

闭包是指有权访问另外一个函数作用域中的变量的函数

JavaScript 代码的整个执行过程，分为两个阶段，代码编译阶段与代码执行阶段。编译阶段由编译器完成，将代码翻译成可执行代码，这个阶段作用域规则会确定。执行阶段由引擎完成，主要任务是执行可执行代码，执行上下文在这个阶段创建。

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/c508a8bbade94a699d0baad47e5d43ed~tplv-k3u1fbpfcp-zoom-1.image)

### 什么是作用域？

ES5 中只存在两种作用域：全局作用域和函数作用域。在 JavaScript 中，我们将作用域定义为一套规则，这套规则用来管理引擎如何在当前作用域以及嵌套子作用域中根据标识符名称进行变量（变量名或者函数名）查找

### 什么是作用域链？

首先要了解作用域链，当访问一个变量时，编译器在执行这段代码时，会首先从当前的作用域中查找是否有这个标识符，如果没有找到，就会去父作用域查找，如果父作用域还没找到继续向上查找，直到全局作用域为止,，而作用域链，就是有当前作用域与上层作用域的一系列变量对象组成，它保证了当前执行的作用域对符合访问权限的变量和函数的有序访问。

### 闭包产生的本质

当前环境中存在指向父级作用域的引用

### 什么是闭包

闭包是一种特殊的对象，它由两部分组成：执行上下文（代号 A），以及在该执行上下文中创建的函数 （代号 B），当 B 执行时，如果访问了 A 中变量对象的值，那么闭包就会产生，且在 Chrome 中使用这个执行上下文 A 的函数名代指闭包。

### 一般如何产生闭包

- 返回函数
- 函数当做参数传递

### 闭包的应用场景

- 柯里化 bind
- 模块

### 参考文章

- [https://segmentfault.com/a/1190000012646221](https://segmentfault.com/a/1190000012646221)

## 问：NaN 是什么，用 typeof 会输出什么？

Not a Number，表示非数字，typeof NaN === 'number'

## （2）问：JS 隐式转换，显示转换

一般非基础类型进行转换时会先调用 valueOf，如果 valueOf 无法返回基本类型值，就会调用 toString

### 字符串和数字

- "+" 操作符，如果有一个为字符串，那么都转化到字符串然后执行字符串拼接
- "-" 操作符，转换为数字，相减 (-a, a \* 1 a/1) 都能进行隐式强制类型转换

```javascript
[] + {} 和 {} + []
```

### 布尔值到数字

- 1 + true = 2
- 1 + false = 1

### 转换为布尔值

- for 中第二个
- while
- if
- 三元表达式
- || （逻辑或） && （逻辑与）左边的操作数

### 符号

- 不能被转换为数字
- 能被转换为布尔值（都是 true）
- 可以被转换成字符串 "Symbol(cool)"

### 宽松相等和严格相等

宽松相等允许进行强制类型转换，而严格相等不允许

#### 字符串与数字

转换为数字然后比较

#### 其他类型与布尔类型

- 先把布尔类型转换为数字，然后继续进行比较

#### 对象与非对象

- 执行对象的 ToPrimitive(对象）然后继续进行比较

#### 假值列表

- undefined
- null
- false
- +0, -0, NaN
- ""

## （2）问：了解 this 嘛，bind，call，apply 具体指什么

它们都是函数的方法

`call: Array.prototype.call(this, args1, args2])`
`apply: Array.prototype.apply(this, [args1, args2])` ：ES6 之前用来展开数组调用, `foo.appy(null, [])`，ES6 之后使用 ... 操作符

- New 绑定 > 显示绑定 > 隐式绑定 > 默认绑定
- 如果需要使用 bind 的柯里化和 apply 的数组解构，绑定到 null，尽可能使用 Object.create(null) 创建一个 DMZ 对象

四条规则：

- 默认绑定，没有其他修饰（bind、apply、call)，在非严格模式下定义指向全局对象，在严格模式下定义指向 undefined

```javascript
function foo() {
  console.log(this.a);
}

var a = 2;
foo();
```

- 隐式绑定：调用位置是否有上下文对象，或者是否被某个对象拥有或者包含，那么隐式绑定规则会把函数调用中的 this 绑定到这个上下文对象。而且，对象属性链只有上一层或者说最后一层在调用位置中起作用

```javascript
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo,
};

obj.foo(); // 2
```

- 显示绑定：通过在函数上运行 call 和 apply ，来显示的绑定 this

```javascript
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
};

foo.call(obj);
```

显示绑定之硬绑定

```javascript
function foo(something) {
  console.log(this.a, something);

  return this.a + something;
}

function bind(fn, obj) {
  return function () {
    return fn.apply(obj, arguments);
  };
}

var obj = {
  a: 2,
};

var bar = bind(foo, obj);
```

New 绑定，new 调用函数会创建一个全新的对象，并将这个对象绑定到函数调用的 this。

- New 绑定时，如果是 new 一个硬绑定函数，那么会用 new 新建的对象替换这个硬绑定 this，

```javascript
function foo(a) {
  this.a = a;
}

var bar = new foo(2);
console.log(bar.a);
```

### （4）问：手写 bind、apply、call

```javascript
// call

Function.prototype.call = function (context, ...args) {
  context = context || window;

  const fnSymbol = Symbol("fn");
  context[fnSymbol] = this;

  context[fnSymbol](...args);
  delete context[fnSymbol];
};
```

```javascript
// apply

Function.prototype.apply = function (context, argsArr) {
  context = context || window;

  const fnSymbol = Symbol("fn");
  context[fnSymbol] = this;

  context[fnSymbol](...argsArr);
  delete context[fnSymbol];
};
```

```javascript
// bind

Function.prototype.bind = function (context, ...args) {
  context = context || window;
  const fnSymbol = Symbol("fn");
  context[fnSymbol] = this;

  return function (..._args) {
    args = args.concat(_args);

    context[fnSymbol](...args);
    delete context[fnSymbol];
  };
};
```

## （3）问：`setTimeout(fn, 0)`多久才执行，Event Loop

setTimeout 按照顺序放到队列里面，然后等待函数调用栈清空之后才开始执行，而这些操作进入队列的顺序，则由设定的延迟时间来决定

## 手写题：Promise 原理

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

## 问：js 脚本加载问题，async、defer 问题

- 如果依赖其他脚本和 DOM 结果，使用 defer
- 如果与 DOM 和其他脚本依赖不强时，使用 async

### 参考资料

- [https://mp.weixin.qq.com/s/pw5lfFeNagmjFj45ygl2dQ](https://mp.weixin.qq.com/s/pw5lfFeNagmjFj45ygl2dQ)

## 问：如何判断一个对象是不是空对象？

Object.keys(obj).length === 0

手写题：在线编程，getUrlParams(url,key); 就是很简单的获取 url 的某个参数的问题，但要考虑边界情况，多个返回值等等

## 问： <script src=’xxx’ ’xxx’/>外部 js 文件先加载还是 onload 先执行，为什么？

onload 是所以加载完成之后执行的

## 问：怎么加事件监听，两种

onclick 和 addEventListener

## 问：事件传播机制（事件流）

冒泡和捕获

## （4）问：说一下原型链和原型链的继承吧

- 所有普通的 [[Prototype]] 链最终都会指向内置的 Object.prototype，其包含了 JavaScript 中许多通用的功能
- 为什么能创建 “类”，借助一种特殊的属性：所有的函数默认都会拥有一个名为 prototype 的共有且不可枚举的属性，它会指向另外一个对象，这个对象通常被称为函数的原型

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.constructor = Person;
```

- 在发生 new 构造函数调用时，会将创建的新对象的 [[Prototype]] 链接到 Person.prototype 指向的对象，这个机制就被称为原型链继承
- 方法定义在原型上，属性定义在构造函数上

- 首先要说一下 JS 原型和实例的关系：每个构造函数 （constructor）都有一个原型对象（prototype），这个原型对象包含一个指向此构造函数的指针属性，通过 new 进行构造函数调用生成的实例，此实例包含一个指向原型对象的指针，也就是通过 [[Prototype]] 链接到了这个原型对象
- 然后说一下 JS 中属性的查找：当我们试图引用实例对象的某个属性时，是按照这样的方式去查找的，首先查找实例对象上是否有这个属性，如果没有找到，就去构造这个实例对象的构造函数的 prototype 所指向的对象上去查找，如果还找不到，就从这个 prototype 对象所指向的构造函数的 prototype 原型对象上去查找
- 什么是原型链：这样逐级查找形似一个链条，且通过   [[Prototype]] 属性链接，所以被称为原型链
- 什么是原型链继承，类比类的继承：当有两个构造函数 A 和 B，将一个构造函数 A 的原型对象的，通过其 [[Prototype]] 属性链接到另外一个 B 构造函数的原型对象时，这个过程被称之为原型继承。

### 标准答案更正确的解释

**什么是原型链？**

当对象查找一个属性的时候，如果没有在自身找到，那么就会查找自身的原型，如果原型还没有找到，那么会继续查找原型的原型，直到找到 Object.prototype 的原型时，此时原型为 null，查找停止。
这种通过 通过原型链接的逐级向上的查找链被称为原型链

**什么是原型继承？**

一个对象可以使用另外一个对象的属性或者方法，就称之为继承。具体是通过将这个对象的原型设置为另外一个对象，这样根据原型链的规则，如果查找一个对象属性且在自身不存在时，就会查找另外一个对象，相当于一个对象可以使用另外一个对象的属性和方法了。

### 参考链接

- [https://zhuanlan.zhihu.com/p/35790971](https://zhuanlan.zhihu.com/p/35790971)

## 问：说下对 JS 的了解吧

是基于原型的动态语言，主要独特特性有 this、原型和原型链。

JS 严格意义上来说分为：语言标准部分（ECMAScript）+ 宿主环境部分

### 语言标准部分

2015 年发布 ES6，引入诸多新特性使得能够编写大型项目变成可能，标准自 2015 之后以年号代号，每年一更

### 宿主环境部分

- 在浏览器宿主环境包括 DOM + BOM 等
- 在 Node，宿主环境包括一些文件、数据库、网络、与操作系统的交互等

## 问：数组能够调用的函数有那些？

- push
- pop
- splice
- slice
- shift
- unshift
- sort
- find
- findIndex
- map/filter/reduce 等函数式编程方法
- 还有一些原型链上的方法：toString/valudOf

## 问：如何判断数组类型

Array.isArray

## 问：  函数中的 arguments 是数组吗？类数组转数组的方法了解一下？

是类数组，是属于鸭子类型的范畴，长得像数组，

- ... 运算符
- Array.from
- Array.prototype.slice.apply(arguments)

## 问：用过 TypeScript 吗？它的作用是什么？

为 JS 添加类型支持，以及提供最新版的 ES 语法的支持，是的利于团队协作和排错，开发大型项目

## 问：PWA 使用过吗？serviceWorker 的使用原理是啥？

`渐进式网络应用（PWA）`是谷歌在 2015 年底提出的概念。基本上算是 web 应用程序，但在外观和感觉上与`原生app`类似。支持`PWA`的网站可以提供脱机工作、推送通知和设备硬件访问等功能。

`Service Worker`是浏览器在后台独立于网页运行的脚本，它打开了通向不需要网页或用户交互的功能的大门。 现在，它们已包括如推送通知和后台同步等功能。 将来，`Service Worker`将会支持如定期同步或地理围栏等其他功能。 本教程讨论的核心功能是拦截和处理网络请求，包括通过程序来管理缓存中的响应。

### 参考链接

- [https://juejin.im/post/5e26aa785188254c257c462d#heading-8](https://juejin.im/post/5e26aa785188254c257c462d#heading-8)

## 问：ES6 之前使用 prototype 实现继承

Object.create() 会创建一个 “新” 对象，然后将此对象内部的 [[Prototype]] 关联到你指定的对象（Foo.prototype）。Object.create(null) 创建一个空 [[Prototype]] 链接的对象，这个对象无法进行委托。

```javascript
function Foo(name) {
  this.name = name;
}

Foo.prototype.myName = function () {
  return this.name;
};

// 继承属性，通过借用构造函数调用
function Bar(name, label) {
  Foo.call(this, name);
  this.label = label;
}

// 继承方法，创建备份
Bar.prototype = Object.create(Foo.prototype);

// 必须设置回正确的构造函数，要不然在会发生判断类型出错
Bar.prototype.constructor = Bar;

// 必须在上一步之后
Bar.prototype.myLabel = function () {
  return this.label;
};

var a = new Bar("a", "obj a");

a.myName(); // "a"
a.myLabel(); // "obj a"
```

## 问:如果一个构造函数，bind 了一个对象，用这个构造函数创建出的实例会继承这个对象的属性吗？为什么？

不会继承，因为根据 this 绑定四大规则，new 绑定的优先级高于 bind 显示绑定，通过 new 进行构造函数调用时，会创建一个新对象，这个新对象会代替 bind 的对象绑定，作为此函数的 this，并且在此函数没有返回对象的情况下，返回这个新建的对象

## (3)箭头函数和普通函数有啥区别？箭头函数能当构造函数吗？

- 普通函数通过 function 关键字定义， this 无法结合词法作用域使用，在运行时绑定，只取决于函数的调用方式，在哪里被调用，调用位置。（取决于调用者，和是否独立运行）
- 箭头函数使用被称为 “胖箭头” 的操作 `=>` 定义，箭头函数不应用普通函数 this 绑定的四种规则，而是根据外层（函数或全局）的作用域来决定 this，且箭头函数的绑定无法被修改（new 也不行）。
  - 箭头函数常用于回调函数中，包括事件处理器或定时器
  - 箭头函数和 var self = this，都试图取代传统的 this 运行机制，将 this 的绑定拉回到词法作用域
  - 没有原型、没有 this、没有 super，没有 arguments，没有 new.target
  - 不能通过 new 关键字调用
    - 一个函数内部有两个方法：[[Call]] 和 [[Construct]]，在通过 new 进行函数调用时，会执行 [[construct]] 方法，创建一个实例对象，然后再执行这个函数体，将函数的 this 绑定在这个实例对象上
    - 当直接调用时，执行 [[Call]] 方法，直接执行函数体
    - 箭头函数没有 [[Construct]] 方法，不能被用作构造函数调用，当使用 new 进行函数调用时会报错。

```javascript
function foo() {
  return (a) => {
    console.log(this.a);
  };
}

var obj1 = {
  a: 2,
};

var obj2 = {
  a: 3,
};

var bar = foo.call(obj1);
bar.call(obj2);
```

### 参考资料

- [https://segmentfault.com/a/1190000015162781](https://segmentfault.com/a/1190000015162781)

## 问：知道 ES6 的 Class 嘛？Static 关键字有了解嘛

为这个类的函数对象直接添加方法，而不是加在这个函数对象的原型对象上

## （3）问：事件循环机制 （Event Loop）

事件循环机制从整体上告诉了我们 JavaScript 代码的执行顺序
`Event Loop`即事件循环，是指浏览器或`Node`的一种解决`javaScript`单线程运行时不会阻塞的一种机制，也就是我们经常使用**异步**的原理。

先执行 Script 脚本，然后清空微任务队列，然后开始下一轮事件循环，继续先执行宏任务，再清空微任务队列，如此往复。

- 宏任务：Script/setTimeout/setInterval/setImmediate/ I/O / UI Rendering
- 微任务：process.nextTick()/Promise

上诉的 setTimeout 和 setInterval 等都是任务源，真正进入任务队列的是他们分发的任务。

### 优先级

- setTimeout = setInterval 一个队列
- setTimeout > setImmediate
- process.nextTick > Promise

```javascript
for (const macroTask of macroTaskQueue) {
  handleMacroTask();
  for (const microTask of microTaskQueue) {
    handleMicroTask(microTask);
  }
}
```

### 参考链接

- [https://juejin.im/post/59e85eebf265da430d571f89](https://juejin.im/post/59e85eebf265da430d571f89)

## （2）手写题：数组扁平化

```javascript
function flatten(arr) {
  let result = [];

  for (let i = 0; i < arr.length; i++) {
    if (Array.isArray(arr[i])) {
      result = result.concat(flatten(arr[i]));
    } else {
      result = result.concat(arr[i]);
    }
  }

  return result;
}

const a = [1, [2, [3, 4]]];
console.log(flatten(a));
```

## 手写题：实现柯里化

预先设置一些参数

柯里化是什么：是指这样一个函数，它接收函数 A，并且能返回一个新的函数，这个新的函数能够处理函数 A 的剩余参数

```javascript
function createCurry(func, args) {
  var argity = func.length;
  var args = args || [];

  return function () {
    var _args = [].slice.apply(arguments);
    args.push(..._args);

    if (args.length < argity) {
      return createCurry.call(this, func, args);
    }

    return func.apply(this, args);
  };
}
```

## 手写题：数组去重

```javascript
Array.from(new Set([1, 1, 2, 2]));
```

## 问：let 闭包

let 会产生临时性死区，在当前的执行上下文中，会进行变量提升，但是未被初始化，所以在执行上下文执行阶段，执行代码如果还没有执行到变量赋值，就引用此变量就会报错，此变量未初始化。

## 问：变量提升

函数在运行的时候，会首先创建执行上下文，然后将执行上下文入栈，然后当此执行上下文处于栈顶时，开始运行执行上下文。

在创建执行上下文的过程中会做三件事：创建变量对象，创建作用域链，确定 this 指向，其中创建变量对象的过程中，首先会为 arguments 创建一个属性，值为 arguments，然后会扫码 function 函数声明，创建一个同名属性，值为函数的引用，接着会扫码 var 变量声明，创建一个同名属性，值为 undefined，这就是变量提升。

## instance 如何使用

左边可以是任意值，右边只能是函数

```javascript
"hello tuture" instanceof String; // false
```

## 参考资料

- [https://juejin.im/post/5d79ccf85188254bf34fd9d1](https://juejin.im/post/5d79ccf85188254bf34fd9d1)
- [https://mp.weixin.qq.com/s/pw5lfFeNagmjFj45ygl2dQ](https://mp.weixin.qq.com/s/pw5lfFeNagmjFj45ygl2dQ)
- [https://mp.weixin.qq.com/s/bHclDpsGdfaZQT8u9VRAAw](https://mp.weixin.qq.com/s/bHclDpsGdfaZQT8u9VRAAw)
- [https://www.jianshu.com/p/cd3fee40ef59](https://www.jianshu.com/p/cd3fee40ef59)

## ❤️ 谢谢支持

喜欢的话别忘了 分享、点赞、在看 三连哦~。
