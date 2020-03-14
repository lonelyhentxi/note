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