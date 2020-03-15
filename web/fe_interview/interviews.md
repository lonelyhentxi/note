# JS Interview

## 用 JS 描述一棵树

```js
// 二叉树
class TreeNode {
    constructor(value, left, right) {
        this.value = value;
        this.left = left;
        this.right = right;
    }
}

// 多叉树
class TreeNode {
    constructor(value) {
        this.value = value;
        this.children = [];
    }
}
```

## 非递归遍历树

队列

## Js New 操作

新建一个对象，将调用函数的 this 指向新建的这个对象

## 方法调用 this 指向

词法作用域

对于全局函数，指向 global/window; 对于方法，指向当前调用的对象；对于 lambda，指向创建时捕获的外围调用者；对于使用 bind、call、apply ，指向新绑定的对象（非严格模式下，绑定非对象会被提升为对象）；对于 new 的函数，指向新创建的对象。

## 什么是 JS 闭包

创建函数式，捕获创建的外围词法作用域的对象的值（基本值类型）和引用（对象），从而可以从内部函数访问外围函数作用域。

## 跨域访问

当一个资源从与该资源本身所在不同的域、协议或者端口请求一个资源时，称为跨域访问。

## Vue 的父子组件之间的通信方式

1. 父组件向子组件 props，单向数据流
2. 子组件向父组件 emit event, 事件

children:

```vue
props: {
    msg: {
        type: String,
        default: () => {
            return ' ';
        }
    }
},
methods: {
    say() {
        this.$emit('say', this.data1);
    }
}
```

parent:

```vue
<children :msg=msg @say=parentSay>
```


此外父组件可以通过子组件的 refs 触发子组件的事件，还可以使用 VueX 和自己写总线的方式进行沟通。

```vue
<children ref="childrenRef"></children>
//...
this.$refs.childrenRef.func()
```

## 用 CSS 写无限循环动画

```css
@keyframes example {
    from { background: red; }
    to { background: blue; }
}

#example1 {
    width: 10px;
    height: 10px;
    animation-timing-function: ease-in;
    animation-name: example;
    animation-duration: 1s;
    animation-iteration-count: infinite;
}
```

## 如何响应式布局

- 媒体查询
    - 设置 viewport
    - 选择屏幕大小分割点
    - 针对不同的范围和类型的设备，复用部分资源，设置不同的网页
- 百分比布局
    - 计算困难
    - 复杂：position 类型，元素类型和属性类型都可能造成负面影响，有副作用
- rem 布局
    - 思想：
        1. 一般不给元素设置具体的宽度
        2. 高度可以按照设计图固定值设计
        3. 所有设置固定的时候用 rem 做单位
        4. JS 设置真实屏幕的宽度，除以设计稿的宽度，作出比例
    - 缺点：
        1. 必须通过 JS 来动态控制根 font-size 大小，存在 JS 和 CSS 的耦合性
- 视口单位：
    - 思想：使用 vm 和 vh，vmin 和 vmax 来布局
- 图片响应式
    - srcset/src
    - background-image
    - picture
- 成型方案：
    - flex
    - grid
    - columns

## 清除 float

1. 添加新元素，应用 clear: both
2. 父级 div 定义 overflow: auto 或者 hidden（根据 BFC 的规则，计算 BFC 高度时，浮动也参与计算）
3. 使用 :after 伪类

```css
.clear{clear:both; height: 0; line-height: 0; font-size: 0}
```

```css
.over-flow{
    overflow: auto; zoom: 1; // zoom 是在处理兼容性问题
}
```

```css
.outer:after {
    clear: both;
    content: '';
    display: block;
    width: 0;
    height: 0;
    visibility: hidden;
}
```

## 模拟 call

假设为浏览器，非严格模式：

```js
Function.prototype.myCall = function(context, ...args) {
    context = (context??window) || new Object(context);
    const key = Symbol();
    context[key] = this;
    const result = context[key](...args);
    delete context[key];
    return result; 
}
```

非浏览器，则 window 改为 global；严格模式，context 不需要判断和 new Object 转换，而在下文调用时注意判断 null 的情况；

## 模拟 apply

假设为浏览器，非严格模式：

```js
Function.prototype.myApply = function(context, args) {
    context = (context??window) || new Object(context);
    const key = Symbol();
    context[key] = this;
    args = args ? args: [];
    // 应对类数组对象
    if(!(args instanceof Array)){
        const newArgs = [];
        for(let i=0;i<args.length;i++) {
            newArgs.push(args[i]);
        }
        args = newArgs;
    }
    const result = context[key](...args);
    delete context[key];
    return result;
}
```

非浏览器和严格模式见上文

## 模拟 bind

假设为浏览器，非严格模式：

```js
Function.propotype.myBind  = function(context, ...args) {
    const fn = this;
    const bindFn = function(...newFnArgs) {
        return fn.call(
            // 应对被当做构造函数使用
            this instanceof bindFn ? this : context,
            , ...args, ...newFnArgs)
    }
    bindFn.prototype = Object.create(fn.prototype);
    return bindFn;
}
```

## 模拟 new

假设为非严格模式：

```js
const createNew = (Con, ...args) => {
    const obj = {};
    Object.setPrototypeOf(obj, Con.prototype);
    const result = Con.apply(obj, args);
    return result instanceof Object ? result : obj;
}
```

## 模拟 instanceof

```js
const myInstanceOf = (left, right) => {
    let leftValue = Object.getPrototypeOf(left);
    let rightValue = Object.prototype;
    // Object.prototype === null
    while(true) {
        if(leftValue===null) {
            return false;
        }
        if(leftValue===rightValue) {
            return true;
        }
        leftValue = Object.getPrototypeOf(leftValue);
    }
}
```

## 手写 AJAX

```js
function formateData(data) {
    const arr = [];
    for(let key in data) {
        arr.push(`${encodeURIComponent(key)}=${data[key]}`);
    }
    return arr.join('&');
}

function ajax(params) {
    params = params || {};
    params.data = params.data || {};
    params.type = (params.type || 'GET').toUpperCase();
    params.data = formateData(params.data);
    const xhr = new XMLHttpRequest();
    return new Promise((resolve, reject)=>{
        xhr.onload = function() {
            if(xhr.status===200 || xhr===304 || xhr.status===206) {
                const res = JSON.parse(xhr.responseText);
                if(params.success && params.success instanceof Function) {
                    params.success.call(xhr, res);
                }
                resolve(res);
            } else {
                const res = JSON.parse(xhr.responseText);
                if(params.error && params.error instanceof Function) {
                    params.error.call(xhr, res);
                }
                reject(res);
            }
    }
    })
    if(params.type==='GET') {
        xhr.open(params.type, `${params.url}?${params.data}`, true);
        xhr.send();
    }
    else if(params.type==='POST') {
        xhr.open(params.type, params.url, true);
        xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
        xhr.send(params.data);
    }
}
```

## 手写 JSONP

```js 
function formateData(data) {
    const arr = [];
        for (let key in data) {
        arr.push(encodeURIComponent(key) + '=' + encodeURIComponent(data[key]))
    }
    return arr.join('&');
}

function jsonp(params) {
    params = params || {};
    params.data = params.data || {};
    const callbackName = params.jsonp;
    const head = document.querySelector('head');
    const script = document.createElement('script');
    params.data['callback'] = callbackName;
    const data = formateData(params.data);
    script.src = `${params.url}?${data}`;
    window[callbackName] = (jsonData) => {
        head.removeChild(script);
        clearTimeout(script.timer);
        delete window[callbackName];
        params.success && params.success(jsonData);
    }
    if(params.time) {
        script.timer = setTimeout(()=>{
            window[callbackName] = null;
            header.removeChild(script);
            params.error && params.error({
                message: 'time out'
            });
        }, params.time);
    }
    head.appendChild(script);
}
```

## 为什么禁止跨域

- 使用 drawImage 等 canvas 绘制函数、以及 WebGL 贴图，可能存在攻击的风险：img、video 等方式访问第三方网页，会带上 cookie
- 使用 XMLHttpRequest 和 Fetch 存在攻击的风险：XSS、CSRF
- 使用 Web 字体，存在攻击的风险：二进制攻击和 CDN 滥用
- 使用 iframe，存在攻击的风险：ClickJacking - 利用嵌入到你的文档或者将你的文档嵌入它的文档，以 iframe 的形式
- 本地数据访问 - Cookie、Storage、indexDB

## OSI 七层

1. 物理层：建立、维护和断开物理连接
2. 数据链路层：建立逻辑连接、硬件寻址、差错检验
3. 网络层：端到端逻辑地址寻址，实现不同网络之间的路径选择
4. 传输层：端口到端口的数据传输，可能有流控、数据校验等其他功能
5. 会话层：建立、管理和终止会话
6. 表示层：数据的标识、安全和压缩
7. 应用层：网络服务于用户的接口

## TCP 三次握手、四次挥手

### 三次握手

LISTEN -> SYSSENT -> ESTAB
LISTEN -> SYN RCVD ->ESTAB

1. Client -> Server SYN, seq=x：客户端发送网络包，服务器收到；服务端确认客户端发送能力和服务端接收能力
2. Server -> Client SYN + ACK, ack=x+1, seq=y：服务端发送网络报，客户端收到；客户端确认自己的发送能力、服务器接收能力、服务器发送能力、客户端接收能力
3. Client -> Server ACK, ack=y+1, seq=x+n：客户端发送网络报，服务器收到；服务端确认服务端发送能力和客户端接收能力

服务端第一次收到 SYN 后，就会处于 SYN_RCVD 状态，称为半连接状态；若发送完 SYN-ACK 包，未收到回复，会指数重传，若超出最大重传次数，将会将半连接队列中删除。初始序号 ISN 会通过一定算法（计数器）随机选择，动态生成，避免攻击。第一次不发送数据，避免攻击。

SYN 攻击是发送很多 SYN，使得正常的 SYN 的半连接队列因为已满而被丢弃，防御方法为：

- 缩短 SYN Timeout 世间
- 增大最大半连接数
- 过滤网关防护

### 四次挥手

客户端 ESTAB -> FIN_WAIT_1 -> FIN_WAIT_2 -> TIMED_WAIT -> CLOSED
服务端 ESTAB -> CLOSE_WAIT -> LAST_ACK -> CLOSED

1. Client -> Server FIN, seq=x：客户端发送一个网络包，服务器收到，确认第 x 个包结束
2. Server -> Client ACK, ACKnum = x+1：服务端发送一个网络包，客户端收到，确认第 x 个包已经被服务器收到
3. Server -> Client FIN, seq=y：服务端发送一个网络包，客户端收到，确认第 y 个包收到
4. Client -> Server ACK ACKnum = y+1: 客户端发送一个网络包，服务器收到，确认客户端第 y 个包收到，同意断开，此时客户端要等待计时器设置的 2MSL （报文段最大生存时间）后才进入 CLOSED 状态，等待服务器未收到确认的重传，同时防止下一个连接中不会出现这种就得请求报文段

两方都要通知对方自己已经结束需要断开，并等待对方同意

## setTimeout 在单线程的 JS 里异步执行

任务队列分为同步任务和异步任务队列；异步任务会被交给事件循环实现，有些异步任务，如 node.js 和浏览器中的 IO 会交给线程池，而一些会造成不可知结果的任务，如浏览器中的渲染则是和 JS 线程交替执行

两种异步任务队列：

宏任务：

1. IO
2. setTimeout
3. setInterval
4. setImmediate
5. requestAnimationFrame

微任务：

1. process.nextTick
2. Promise
3. Promise.then
4. MutationObserver

宏任务执行后，首先将微任务队列执行完毕后，再执行下一个宏任务。创建异步任务可能有定时器模块，到了执行时间才开始将异步任务加入异步队列。

## var 和 let、const 的区别

var 是函数作用域, 不允许变量提升，不存在死区；let、const 是词法作用域，不允许变量提升，存在死区，不允许重复声明

## 进程和线程的区别

进程是包含独立资源上下文的 CPU 工作时间单位，线程是共享进程资源上下文的 CPU 工作时间单位。

## JS 绑定新对象

bind, apply, call

## 手写快排

```js
function partition(arr, l, r) {
    const pivot = arr[l];
    while(l<r) {
        while(arr[r] >= pivot && l < r) {
            r--;
        }
        if(l<r) {
            arr[l] = arr[r];
            l++;
        }
        while(arr[l] <= temp && l < r) {
            l++;
        }
        if(l<r) {
            arr[r] = arr[l];
            r--;
        }
    }
    arr[l] = pivot;
    return l;
}

function quickSort(arr, l, r) {
    if(l<r) {
        const boundary = partition(arr, l, r);
        quickSort(arr, l, boundary);
        quickSort(arr, boundary + 1, r);
    }
}
```

## http 状态码

### 200

200 OK 请求成功 
201 Created 资源创建：POST、PUT
202 Accepted 请求收到，但未响应：异步无回调
204 No Content 成功处理请求，但不需要返回实体内容：如元信息的更新，缓存存活
205 Reset Content 请求者重置文档视图
206 Partial Content 部分请求成功

### 300

300 Multiple Choice 请求的资源有一系列可供选择的回馈信息，各有特定的地址和浏览器驱动的商议信息
301 Move Permanently 永久移动到新位置
302 Found 请求的资源现在从不同的 URI 响应请求
303 See Other 对应当前的请求可以在另一个 URI 上找到，且客户端应该使用 GET 方式访问
304 Not Modified 请求了带条件的 GET 已被允许，但文档的内容相对上次没有变化

### 400

400 Bad Request 语义有误；或者请求参数有误
401 Unauthorized 当前请求需要用户验证
403 Forbidden 服务器已经理解请求，但是拒绝执行，可能是权限不足等
404 Not Found 请求失败，该资源未在服务器上发现
405 Method Not Allowed 该方法不能用于请求对应资源
406 Not Acceptable 资源的特性不满足头的要求
407 Proxy Authentication Required 客户端必须在代理服务器上进行身份验证
408 Request Timeout 请求超时
409 Conflict 被请求的资源和当前状态之间存在冲突，如重复主键的用户
410 Gone 资源永久不再可用，如被删除
411 Length Required 需要长度
412 Precondition Failed 在验证请求字段中先决条件时，一个或者多个没有满足
413 Payload Too Large 服务器拒绝处理当前请求
414 URL Too Long
415 Unsupported Media Type 提交了不支持的媒体类型
426 Upgrade Required 服务器升级后可能能够执行当前请求
428 Precondition Required 原始服务器的请求有条件，旨在防止丢失更新
429 Too Many Requests 在给定时间中发送了太多请求
431 Request Header Fields Too Large 请求头太大
451 Unavailable For Legal Reasons 用户请求非法资源

### 500

500 Internal Server Error 服务器内部错误
501 Not Implement 此方法不被服务器支持且无法处理
502 Bad Gateway 服务器作为网关需要得到一个处理这个请求的响应
503 Service Unavailable 服务器没有准备好请求，可能是服务器在维护或者重载而停机
504 Gateway Timeout 网关服务器不能及时得到相应

## 死锁的四要素和定义

死锁定义：两个或者以上的进程在执行过程中，因资源争夺而造成的互相等待的现象，若无外力作用，无法继续推进。

死锁原因：

1. 系统资源不足
2. 请求顺序不适当
3. 资源分配不当

四要素：

1. 循环等待
2. 请求并保持
3. 互斥资源
4. 不可剥夺

死锁的解除和预防：

1. 预防
    - 优先级
    - 尝试获取的超时
    - 可抢占
    - 可共享资源不要设置为互斥
2. 避免
    - 进程启动拒绝
    - 资源分配拒绝
3. 解除
    - 剥夺资源
    - 撤销进程

## 同源的定义

同协议，同抽象主机，同端口号

## cookie 和 session 的区别

cookie 是客户端中保存少量信息的机制，用于记录用户的信息，也是实现 session 的一种方法

session 是开发者为了实现中断和继续等操作，将 user agent 和 service 之间的交互，抽象为的会话及其会话状态。常见实现方式为，服务端中保存的数据结构，用来记录用户的状态，也可以通过 JWT 的方式实现

## ES5 和 ES6 中的作用域和闭包

ES5 的 var，全局和函数级
ES6 词法块级作用域
函数化的闭包，实现为对值对象获取其当时值快照、引用对象获取引用指针保存在创建的匿名对象中，通过类似 operator() 的方法予以调用

## 输入 URL 到渲染完成

域名解析-TCP分包-IP寻路-握手-滑动窗口传输-持久化连接-挥手-解析-构建dom树与cssom-构建渲染树-回流-重绘-渲染

@TODO

## Promise.all 用法和实现

Promise.all(PromiseArray).then() 等待全部任务完成后执行，或者出现第一个失败

```js
function myPromiseAll(arr) {
    return new Promise((resolve, reject)=>{
        const result = new Array(arr.length);
        arr.forEach((p, index)=>{
            Promise.resolve(p).then(d=>{
                result[index] = d;
                count ++;
                if(count===arr.length) {
                    resolve(result);
                }
            }, err=> reject(err));
        });
    });
}
```

### This 的指向

谁调用 this，this 就指向谁

- 全局
    - 浏览器 window
    - node {}
- new
    - 返回对象，指向该对象
    - 返回其它，指向新对象
- call，apply，bind
    - 严格模式下，绑定传入的参数
    - 非严格模式下
        - 若为空，指向全局对象
        - 若为值对象，返回对应的应用对象
        - 其它，为传入的参数
- 方法调用，调用位置上下文的对象
- lambda，外层上下文绑定的 this 

## JS 基本对象

- Boolean
- String
- Null
- Undefined
- Symbol
- BigInt
- Object

**typeof null==='object' JS 最初版本在 32 位环境中，性能考虑，低位存储类型，000 开头表示对象，null 全零，错误判断为 object**

## HTML5 语义化的理解

优点：

- 代码结构清晰，易于理解、开发和维护
- 利于屏幕阅读器和爬虫等其他类型客户代理阅读
- 有益于 SEO

标签：

- title
- header
- nav
- main
- section
- article
- h1-h6
- ul/ol/li
- table/thead/tbody/tfoot/...
- aside
- figure
- picture
- details
- mark
- dialog
- address
- img/audio/video/canvas/picture/source

## 让 (a==1 && a==2 && a==3) 为 true

### 利用隐式转换规则

== 操作符在左右数据类型不一致的时候，会进行隐式转换，转换规则如下：

- 如果部署 `[Symbol.toPrimitive]` 接口，那么调用此接口
- 如果非 Date 类型，调用 valueOf，如果返回非基本数据类型，返回调用 toString，如果 toString 返回非基本类型，那么抛出错误
- 如果 hint 为 string，调用顺序为 toString >>> valueOf
- 如果 hint 为 number，调用顺序为 valueOf >>> toString

```js
const obj = {
    [Symbol.toPrimitive]: (function(hint) {
        let i = 0;
        return () => i++;
    })()
}
```

```js
const obj = {
    reg: /\d/g,
    valueOf: function(hint) {
        return this.reg.exec('123')[0];
    }
}
```

### 利用数据劫持

```js
let i = 1;
Object.defineProperty(window, 'a', {
    get: function() {
        return i++;
    }
});
```

```js
const a = new Proxy({}, {
    i: 1,
    get: function() {
        return () => this.i++;
    }
})
```

### 利用数组的 join 方法

```js
const a = [1, 2, 3];
a.join = a.shift;
```

## 防抖函数作用、应用场景、实现

debounce 每当执行调用之后，更新计时，只有到计时结束时，才能触发动作

1. 调节窗口
2. 字符输入
3. 表单验证

```js
function myDebounce(fn, wait, immediate = true) {
    let timer;
    const later = (context, args) => setTimeout(()=>{
        timer = null;
        if(!immediate) {
            fn.apply(context, args);
        }
    }, wait);
    const debounced = function (...params) {
        const context = this;
        const args = params;
        if(!timer) {
            timer = later(context, args);
            if(immediate) {
                fn.apply(context, args);
            }
        } else {
            clearTimeout(timer);
            timer = later(context, args);
        }
    }
    debounced.cancel = function() {
        clearTimeout(timer);
        timer = null;
    };
    return debounced;
}
```

## 节流的作用、应用场景、实现

throttle 规定一个时间，一段时间内只能执行一次：

1. 按钮点击
2. 拖拽事件
3. 计算鼠标的距离

时间戳实现：

```js
function throttle (func, delay) {
    let lastTime = 0;
    const throttled = throttled(...args) {
        const context = this;
        const nowTime = Date.now();
        if(nowTime > lastTime + delay) {
            func.apply(context, args);
            lastTime = nowTime;
        }
    }
    throttled.cancel = function() {};
    return throttled; 
}
```

计时器实现：

```js
function throttle(func, delay) {
    let timeout = null;
    const throttled = function(...args) {
        const context = this;
        if(!timeout) {
            timeout = setTimeout(()=>{
                func.apply(context, args);
                clearTimeout(timeout);
                timeout=null
            }, delay);
        }
    }
    throttled.cancel = function() {
        clearTimeout(timeout);
        timeout = null;
    };
    return throttled;
}
```

## 说一说对 JS 执行上下文栈和作用域链的理解

JS 执行上下文就是 JavaScript 代码被解析和执行时所在环境的抽象概念，执行上下文分为：

- 全局执行上下文
- 函数执行上下文
- eval 函数执行上下文

创建过程：

1. 创建变量对象：初始化 arguments，提升函数声明和变量声明
2. 创建作用域链：在执行上下文的创建阶段，作用域链在变量创建后创建
3. 确定 this 的值

其栈即为函数调用栈。

作用域负责收集和维护所有声明的标识符（变量）组成的一系列查询，并实施一套严格的规则，确定当前执行的代码对这些标识符的访问权限，JS 使用词法作用域。其作用域链为逐层向外。

## 如何理解 BFC？BFC 的布局规则为？如何创建 BFC？

### 如何理解 BFC

盒子是 CSS 引擎根据文档中内容所创建的，不同类型的盒子在文档上占据不同类型的上下文。格式化上下文是页面的一块渲染区域及其渲染规则，包括它子元素的定位以及其自身与其它盒子的关系。一个块级盒子一定会参与格式化上下文的创建。

### BFC 的布局规则为

- 内部，盒子依次垂直排列
- 两个盒子的距离由 margin 决定，上下相邻的 margin 会发生重叠（其实内部接触也会重叠）
- BFC 内每个盒子的左 margin-left-edge 接触外部盒子的 content-left-edge，即使在左浮动情况下也是如此
- BFC 内的行上下文会自动伸缩以适应两侧浮动
- BFC 内计算好后，不会影响外部元素
- 计算 BFC 高度时，浮动元素也会计算

### BFC

- 根元素
- 浮动元素
- position: absolute 或者 fixed
- display: block, inline block, table-cell, table-caption, inline table, display: flow-root, contain: layout/content/paint 的元素，弹性元素、网格元素、多列元素, overflow: visible
- 匿名块盒子

## 深拷贝

@TODO

## 什么是 XSS 攻击？分为哪几类？怎么防范？

### 什么是 XSS 攻击

cross site scripting，跨站脚本攻击。攻击者在目标网站植入恶意代码，利用用户对网站的信任，当攻击者登录网站时就会执行恶意代码。

### XSS 分类和方法手段

- 反射型 XSS：攻击者构造特殊 URL，包含恶意代码；用户打开具有恶意代码的 URL 时，恶意代码被从 URL 中取出，在客户端浏览器中执行，恶意代码窃取用户数据并发送到攻击者网站或者冒充用户执行指定操作
    - 对字符串进行编码
    - cookie 设置 httpOnly 和 Secury
- DOM 型 XSS：前端 JS 代码不够严谨，将不可信内容插入了页面上
    - 对 url 链接，使用 encodeURIComponent 转义
    - 对 html 内容进行转义
- 存储型 XSS：恶意脚本永久存储在服务器上，被返回并执行
    - 前端传递数据给服务器前，先转义/过滤
    - 服务器收到数据，存储到服务器前，先转义/过滤
    - 前端接收到服务器数据展示前，先转义过滤

其它防范 XSS 攻击的手段：

```config
Content-Security-Policy: default-src 'self'
<meta http-equiv="Content-Security-Policy" content="form-action 'self';">
```

- Content Security Policy，后者无法设置 report
    1. 禁止加载外域代码
    2. 禁止外域提交
    3. 禁止内联脚本执行
    4. 禁止未授权脚本运行
    5. 合理使用上报可以尽快发现 XSS
- 输入长度控制
- 输入内容控制
- 动作确认
- HTTPonly 和 Secure
- 验证码

## 如何隐藏页面中元素

- display: none 不占据空间的完全隐藏
- hidden 属性，CSS3 新增，不占据空间，完全隐藏
- position：absolute/fixed + left/top: -9999999px，向左、上移动，不占空间
- position: relative + left/top: -9999999px，占据空间，若要不占空间，可以设置 height: 0
- margin-left: -999999px; 占据空间
- transform 
    - scale(0) 不占空间
    - translateX/Y(-9999999px) 占据空间，可设置 height
    - rotateX/Y(90deg) 占据空间
- height: 0, width: 0, line-height: 0, font-size: 0, overflow: hidden 不占空间
- opacity: 0 占据空间
- visibility: hidden 占据空间
- z-index: -999 + position: relative 占据空间
- clip-path: polygon(0 0, 0 0, 0 0, 0 0); 占据空间
- aria-hidden 属性，语义上隐藏，占空间可见

## 浏览器的事件代理机制的原因是？

事件流：DOM2 级事件将事件流分为是三个阶段，捕获阶段、目标阶段、冒泡阶段

事件代理机制的原理：在祖先 DOM 绑定一个元素，当触发子孙级 DOM 元素的事件时，利用事件冒泡的原理来触发绑定在祖先级 DOM 的事件，一层层冒泡到 document 对象

事件代理的原因：

1. 减少实际添加到网页上的事件，维护性能
2. 事件代理时，子孙动态，不需要为新添加的子孙绑定事件
3. 子孙移除后，不需要手动清理事件
4. 允许一个事件注册多个监听
5. 提供更精细的手段控制 listener 的触发阶段
6. 对任何 DOM 有效，而不仅仅是 HTML

addEventListener:

```js
target.addEventListener(type, listener[, options]);
target.addEventListener(type, listener[, useCapture]);
```

options 对象

- capture：true 表示在捕获阶段触发，否则在冒泡阶段触发
- once: 在添加后调用一次，后自动移除
- passive: 永远不应该调用 preventDefault() 如果调用，则会忽略并得到控制台警告

## setTimeout 倒计时误差

加入宏队列需要时间，计时到达后等待主线程任务完成需要时间
方法：从服务器或者 ntp 获取时间，修正计时器

## 异步加载 JS 脚本的方法有

### defer/async 属性

```html
<script src="../xxx.js" defer/async></script>
```

- defer: 等到页面在内存中正常渲染结束，其它脚本完成执行，window.onload 之前执行，如果多个 defer，会按照顺序加载
- async：下载完，渲染引擎中断渲染，执行该脚本后渲染，多个 async 不能保证顺序

### preload/prefetch 预加载

```html
<link rel=preload/prefetch href="../xxx" as=media-type type=mime-type>
```

preload 提前获取之后该页面要使用的资源，prefetch 提前获取之后其它页面要使用的资源

### 动态创建 script 标签

```js
let script = document.createElement('script');
script.src = 'XXX.js';
document.body.append(script);
```

### XHR 或者 fetch 异步加载 JS 并使用 eval 执行

```js
let xhr = new XMLHttpRequest();
xhr.open("get", "js/xxx.js",true);
xhr.send();
xhr.onreadystatechange = function() {
    if (xhr.readyState == 4 && xhr.status == 200) {
        eval(xhr.responseText);
    }
}
```

### import() 结合 fetch

## 实现 flattenDeep 函数，将嵌套数组扁平化

### arr.flat

```js
function flattenDeep(arr, deepLength) {
    if(deepLength>Number.MAX_SAFE_INTEGER) {
        deepLength = Number.MAX_SAFE_INTEGER;
    }
    return arr.flat(deepLength);
}
```

### reduce 和 concat

```js
function flattenDeep(arr) {
    return arr.reduce((acc, val)=> Array.isArray(val) ? arr.concat(flattenDeep(val)): acc.concat(val), []);
}
```

### 利用 stack 实现

## 可迭代对象的特点

有 `[Symbol.iterator]` 接口返回一个遍历器对象，可以使用 next() 函数获取 value 和 done，可以使用 for ... of 进行循环

### 自定义迭代器对象

```js
*fn() {
    yield xxx
}
```

使用生成器可以自定义迭代器对象

### 异步迭代器

有 `[Symbol.asyncIterator]` 接口返回一个遍历器对象，可以使用 next() 获取 value 和 done 的 promise，可以使用 for await ... of 循环

异步生成器

```js
async function* fn() {
    yield await xxx;
}
```

## Promise.race 及其实现

只要有一个参数 fullfilled 或者 reject，则 fullfilled 或者 rejected

实现：

```js
function myRace(iterable) {
    const arr = Array.from(iterable);
    return new Promise((resolve, reject)=> {
        if(arr.length===0) {
            return resolve();
        }
        arr.forEach(p=> Promise.resolve(p).then((d)=>resolve(d), err=>reject(err)));
    });
}
```

## 数组去重

```js
function unique(array) {
    return [...new Set(array)];
}
```

其它方法时间复杂度较高

## 编写通用 currying

```js
function currying(fn, ...args) {
    return (...others) => fn(...args, ...others);
}
```

## 原型链继承的思路和优缺点

调用方法和属性时，如果没找到会顺着原型链向上查找，实现某种程度的继承

优点：

1. 单继承
2. 可以像超类传递参数
3. 解决引用值共享问题
4. 可以动态修改原型链

缺点：

1. 单继承
2. 查找耗时

## 组合继承的思路和优缺点

将原型链和借用构造函数结合到一起，通过原型链实现对原型属性和方法的继承，通过借用构造函数实现对实例属性的继承

优点：

1. 每个子类可有自己的属性
2. 可向超类传递参数
3. 可函数复用

缺点：

1. 调用两次构造函数

## 原型式继承

通过已有对象创建新对象，不必创建自定义类型，ES6 中可以使用 Object.create(proto) 来创建

优点：

1. 借助已有对象创建新对象，不必创建自定义类型

缺点：

1. 包含引用类型的属性会被所有实例共享

## 寄生式继承

类似工厂模式

优点：

1. 考虑对象而不是自定义类型和构造函数，很有用

缺点：

1. 不能函数复用
2. 包含引用类型只的属性会被所有实例共享

## 寄生组合式继承

原型链继承方法，借用构造函数继承属性

优点：

只调用一次超类构造函数
原型链不变

## ES6 继承

通过 extends 关键字实现：

```js
console.log(typeof SuperType);//function
console.log(SuperType === SuperType.prototype.constructor); //true
```

类内方法默认不可枚举，而 ES5 可枚举；class 不能不加 new 直接调用；子类继承必须使用 super 方法，否则报错；使用 super 后才能使用 this 关键字

## 实现 `JSON.stringify`

```js
function jsonStringify(data, set) {
    const dataType = typeof data;
    if (dataType!=='object') {
        if(Number.isNaN(data) || data===Infinity || data===-Infinity) {
            return "null";
        } else if(dataType==="function" || dataType==="symbol" || dataType==="undefined") {
            return "undefined";
        } else if(dataType==="string") {
            return `"${data}"`;
        } else if(data.toString && typeof data.toString === "function") {
            // such as bigint
            return data.toString();
        } else {
            // unreachable in es10
            return "undefined";
        }
    } else {
        if(data===null) {
            return "null";
        } else if(data.toJson && typeof data.toJson === "function") {
            return jsonStringify(data.toJSON());
        } else if(data instanceof Array) {
            const result = [];
            data.forEach((item)=>{
                const itemType = typeof item;
                if(itemType === "undefined" || itemType==="function" || itemType === "symbol") {
                    result.push("null");
                } else {
                    result.push(jsonStringify(item));
                }
            });            
            result = `[${result}]`;
            return result.replace(/'/g, '"');
        } else {
            if(set) {
                set = new Set();
            }
            if(set.get(data)!==null) {
                return "undefined";
            }
            set.add(data);
            const result = [];
            Object.keys(data).forEach((item)=>{
                if(typeof item!=='symbol') {
                    if (data[item] !== undefined && typeof data[item] !== 'function'
                        && typeof data[item] !== 'symbol') {
                        result.push('"' + item + '"' + ":" + jsonStringify(data[item], new Set(set)));
                    }
                }
            });
            return ("{" + result + "}").replace(/'/g, '"');
        }
    }
}
```

## 实现 JSON.parse

```js
(new Function('return'+json))();
```

基于 eval，必要时需要做过滤，避免 XSS

## 实现 Observable 和 Subject

@TODO

## 使用 CSS 让一个元素水平垂直居中

### 利用 flex 布局

```css
.container {
    display: flex;
    aligh-items: center;
    justify-content: center;
}
```

### 子元素是单行文本

```css
.container {
    height: 100px;
    line-height: 100px;
    text-align: center;
}
```

### 利用 absolute + transform

```css
.container {
    position: relative;
}

.box {
    top: 50%;
    left: 50%;
    transform: translate(50%, 50%);
}
```

### 利用 grid 布局

```css
.container {
    display: grid;
}

.box {
    justify-self: center;
    align-self: center;
}
```

### 利用绝对定位和 margin: auto

```css
.container {
    position: relative;
}
.box {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 0;
    margin: auto;
}
```

## ES6 模块和 CommonJS 模块的差异

1. 运行时和编译时
    - CommonJS 模块运行时加载，import 是静态加载，编译时确定模块依赖关系；注意有 import() 是动态的
    - CommonJS 加载的是对象，该对象在脚本运行完成才会生成
2. CommonJS 输出的是值的拷贝，ES6 输出的是值的只读引用
3. ES6 模块自动使用严格模式
4. require 可以做动态加载，import 语句（非import函数）做不到
6. 使用 require 加载某个模块时，就会运行整个模块的代码
7. 当使用 require 命令加载同一个模块时，会使用缓存，除非删除缓存，否则不会得到新值

ps: export default 可以理解为将变量赋值给 default，最后导出 default

## 重绘和重排

### 渲染过程

1. DOM Tree -> Render Tree
2. Style struct -> Render Tree
3. Render Tree -> Paint

浏览器解析 HTML 源码，创建 DOM 树，每个 HTML 标签在树上都有一个对应的节点；标签中的文本也有对应的文本节点；DOM 树上根节点式 documentElement；浏览器解析 CSS；合并创建渲染树，类似 DOM 树，但并不一一对应；渲染树上的节点使用对应的格式化上下文进行渲染。一旦渲染树被创建成功，浏览器就可以在屏幕上绘制渲染树节点。

### 重绘和重排

在第一次排列和绘制之后，每次改变用于构建渲染树的信息都会导致至少：

- 部分渲染树的节点尺寸需要重新计算 —— 重排
- 由于节点几何属性或者样式发生改变，更新屏幕内容 —— 重绘

这些操作代价高昂

### 触发的场景

1. 无格式化上下文几何变化的变化 -> 重排和重绘
2. 单纯样式变化 -> 重绘

### 浏览器的优化

创造变化队列，在一定时间内同种可覆盖变化会发生覆盖；但在可能需要几何信息的情况下会立即触发变化，可以先缓存值

### 最小化重排和重绘

- 不要逐个改变样式
- 离线的批量改变和表现 DOM
- 通过 documentFragment 来保留临时改动
- 通过复制改变替换的方式
- 通过 display: none 属性隐藏元素，添加足够多的变化后，再显示
- 不频繁计算样式，如果有需要，可以缓存

## 如何让 Promise.all 在抛出异常后仍然有效

给每个作为参数的 promise 实例添加 catch 方法，则其在抛出异常并经过 catch 处理后会转换为 fullfilled 状态

```js
const tasks = promises.map(p=>Promise.resolve(p).catch(e=>e));
Promise.all(tasks) //...
```

## Promise如何满足多个异步进程的同步顺序

1. then 链式调用，若前一个回调函数返回的是 promise 对象，则后一个回调会等待该 promise 对象的状态发生变化，后者才会被调用
2. async await 语法糖

## Promise 的状态

pending、fullfilled、rejected