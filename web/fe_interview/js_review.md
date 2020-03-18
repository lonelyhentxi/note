1.  数据类型和变量：

    1.  JS 数据类型：

        1.  number：遵循 IEEE 754 标准的双精度 64 位格式；在实现时，整数通常被视为 32 位整形变量

            1.  Math 数学对象，用以处理更多的高级数学函数和常数

            2.  parseInt(string, radix)

            3.  parseFloat(string) 只解析十进制

            4.  NaN 和任何运算都是 NaN，可用 isNaN(var) 来判断

            5.  Infinity 和 -Infinity 表示有穷和无穷，isFinite(var) 可以判断是否有穷

            6.  Number.MAX\_VALUE 和 Number.MIN\_VALUE 可以表示非无穷的数字

            7.  Number.MAX\_SAFE\_INTEGER 和 Number.MIN\_SAFE\_INTEGER 表示在双精度浮点数的取值范围内，以外取值不再安全，可用 Number.isSafeInteger(num) 检查

            8.  BigInt 类型是 JS 的一个基础的数值类型，可以表示任意精度整数，它不能与数字互操作，否则会报 TypeError

        <!-- -->

        1.  object：可以简单地理解为名称-值对

            1.  常见类型：

                1.  Function：见 &lt;函数&gt;

                2.  Array

                3.  Date：new Date() 当前时间，Date() 当前日期字符串

                4.  RegExp

                5.  Error

                6.  BigInt

                7.  TypedArray

                8.  键控集 Map, Set, WeakMap, WeakSet

                9.  结构化数据 JSON

            <!-- -->

            1.  数据属性

                1.  数据属性的特征

                    1.  

<table><thead><tr class="header"><th>[[Value]]</th><th>任何JS类</th><th>包含这个属性的数据值</th><th>undefined</th></tr></thead><tbody><tr class="odd"><td>[[Writable]]</td><td>Boolean</td><td>可否改变</td><td>true</td></tr><tr class="even"><td>[[Enumerable]]</td><td>Boolean</td><td>可否使用 for in 循环</td><td>true</td></tr><tr class="odd"><td>[[Configurable]]</td><td>Boolean</td><td>可否被删除, 除了 [[Value]] 和 [[Writable]] 之外的特性能否改变</td><td>true</td></tr></tbody></table>

1.  访问器属性

<table><thead><tr class="header"><th><strong>特性</strong></th><th><strong>类型</strong></th><th><strong>描述</strong></th><th><strong>默认值</strong></th></tr></thead><tbody><tr class="odd"><td>[[Get]]</td><td>函数对象或者 undefined</td><td>该函数使用一个空的参数列表，能够在有权访问的情况下读取属性值。另见 <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/get"><span class="underline">get</span></a>。</td><td>undefined</td></tr><tr class="even"><td>[[Set]]</td><td>函数对象或者 undefined</td><td>该函数有一个参数，用来写入属性值，另见 <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/set"><span class="underline">set</span></a>。</td><td>undefined</td></tr><tr class="odd"><td>[[Enumerable]]</td><td>Boolean</td><td>如果该值为 true，则该属性可以用 <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...in"><span class="underline">for...in</span></a> 循环来枚举。</td><td>true</td></tr><tr class="even"><td>[[Configurable]]</td><td>Boolean</td><td>如果该值为 false，则该属性不能被删除，并且不能被转变成一个数据属性。</td><td>true</td></tr></tbody></table>

1.  string：Unicode 字符序列

2.  null

3.  undefined

4.  boolean

5.  symbol

    1.  匿名不可枚举

    2.  访问全局 Symbol 注册表方法是 Symbol.For("string") 和 Symbol.KeyFor(symbolValue)

<!-- -->

1.  变量：

    1.  注意：避免全局变量：不使用声明符的变量赋值都是全局变量

    2.  变量声明：

        1.  let 语句声明一个块级作用域的本地变量，可选地初始化为一个值

        2.  const 声明一个顶层不可变的本地常量，必须初始化为一个值

        3.  var 声明的变量在声明的整个函数可变

<!-- -->

1.  typeof

<table><thead><tr class="header"><th><strong>Type</strong></th><th><strong>Result</strong></th></tr></thead><tbody><tr class="odd"><td><a href="https://developer.mozilla.org/en-US/docs/Glossary/Undefined"><span class="underline">Undefined</span></a></td><td>"undefined"</td></tr><tr class="even"><td><a href="https://developer.mozilla.org/en-US/docs/Glossary/Null"><span class="underline">Null</span></a></td><td>"object" (see <a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/typeof#null"><span class="underline">below</span></a>)</td></tr><tr class="odd"><td><a href="https://developer.mozilla.org/en-US/docs/Glossary/Boolean"><span class="underline">Boolean</span></a></td><td>"boolean"</td></tr><tr class="even"><td><a href="https://developer.mozilla.org/en-US/docs/Glossary/Number"><span class="underline">Number</span></a></td><td>"number"</td></tr><tr class="odd"><td><a href="https://developer.mozilla.org/en-US/docs/Glossary/BigInt"><span class="underline">BigInt</span></a> (new in ECMAScript 2020)</td><td>"bigint"</td></tr><tr class="even"><td><a href="https://developer.mozilla.org/en-US/docs/Glossary/String"><span class="underline">String</span></a></td><td>"string"</td></tr><tr class="odd"><td><a href="https://developer.mozilla.org/en-US/docs/Glossary/Symbol"><span class="underline">Symbol</span></a> (new in ECMAScript 2015)</td><td>"symbol"</td></tr><tr class="even"><td><a href="https://developer.mozilla.org/en-US/docs/Glossary/Function"><span class="underline">Function</span></a> object (implements [[Call]] in ECMA-262 terms)</td><td>"function"</td></tr><tr class="odd"><td>Any other object</td><td>"object"</td></tr></tbody></table>

1.  函数：

    1.  正确的函数定义：非变量方式声明的函数是执行任何代码之前进行解析的

    2.  arguments 对象是一个包含位置索引和长度的类数组对象

    3.  this

        1.  指向

            1.  全局环境：Global 对象

            2.  浏览器：window 对象

            3.  new 运算符创建的对象：创建出来的对象

            4.  在 A 函数中定义 B 函数：指向 A 函数的 this

            5.  作为对象属性的函数：指向该对象

            6.  作为静态函数使用的方法：指向外围作用域

            7.  lambda： 创建 lambda 时候捕获的外围环境

        <!-- -->

        1.  指向总结：每次调用一个函数时，该函数的 this 总是指向函数被调用的父作用于提供

    <!-- -->

    1.  函数应用：

        1.  call

            1.  作用：调用一个对象的方法，以另一个对象替换当前的 this 上下文，参数以解包装形式使用

            2.  签名：Function.prototype.call(thisArg\[, arg1 \[, arg2, …\]\])

        <!-- -->

        1.  apply

            1.  作用：调用一个对象的方法，以另一对象替换当前的 this 上下文，参数以可迭代对象形式使用

            2.  签名：Function.prototype.apply(thisArg, argArray)

            3.  如果 apply 方法的第一个参数不是对象，它将成为被调用函数的所有者

    <!-- -->

    1.  length: 待传入非展开和默认参数的长度(writable: false, enumerable: false, configurable)

> console.log(Function.length); /\* 1 \*/
>
> console.log((function() {}).length); /\* 0 \*/  
> console.log((function(a) {}).length); /\* 1 \*/  
> console.log((function(a, b) {}).length); /\* 2 etc. \*/
>
> console.log((function(...args) {}).length);  
> // 0, rest parameter is not counted
>
> console.log((function(a, b = 1, c) {}).length);  
> // 1, only parameters before the first one with  
> // a default value is counted
>
>  

1.  闭包：

    1.  概念：函数对其状态即词法环境的引用共同构成闭包

    2.  特殊用途：模拟私有变量

    3.  注意：

        1.  在与 var 结合使用的时候，注意回调可能共享同一个环境

        2.  闭包在处理速度和内存消耗方面有负面影响

<!-- -->

1.  相等性比较：

    1.  同值相等 Object.is(x,y)

        1.  Object.is 不进行隐式转换

        2.  undefined 相等，null 相等

        3.  true 相等，false 相等

        4.  相同字数且字符按顺序相等的字符串

        5.  指向同一个对象

        6.  都是数字

            1.  都是正 0

            2.  都是负 0

            3.  都是 NaN

            4.  都是除了零 和 NaN 外的其它数字

    <!-- -->

    1.  严格相等 ===

        1.  不进行隐式转换

        2.  自身相等，所有的子孙相等

    <!-- -->

    1.  非严格相等 ==

        1.  隐式转换

<table><thead><tr class="header"><th> </th><th><strong>被比较值 B</strong></th><th> </th><th> </th><th> </th><th> </th><th> </th><th> </th></tr></thead><tbody><tr class="odd"><td> </td><td> </td><td>Undefined</td><td>Null</td><td>Number</td><td>String</td><td>Boolean</td><td>Object</td></tr><tr class="even"><td><strong>被比较值 A</strong></td><td>Undefined</td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>IsFalsy(B)</td></tr><tr class="odd"><td> </td><td>Null</td><td>true</td><td>true</td><td>false</td><td>false</td><td>false</td><td>IsFalsy(B)</td></tr><tr class="even"><td> </td><td>Number</td><td>false</td><td>false</td><td>A === B</td><td>A === ToNumber(B)</td><td>A=== ToNumber(B)</td><td>A== ToPrimitive(B)</td></tr><tr class="odd"><td> </td><td>String</td><td>false</td><td>false</td><td>ToNumber(A) === B</td><td>A === B</td><td>ToNumber(A) === ToNumber(B)</td><td>ToPrimitive(B) == A</td></tr><tr class="even"><td> </td><td>Boolean</td><td>false</td><td>false</td><td>ToNumber(A) === B</td><td>ToNumber(A) === ToNumber(B)</td><td>A === B</td><td>ToNumber(A) == ToPrimitive(B)</td></tr><tr class="odd"><td> </td><td>Object</td><td>false</td><td>false</td><td>ToPrimitive(A) == B</td><td>ToPrimitive(A) == B</td><td>ToPrimitive(A) == ToNumber(B)</td><td>A === B</td></tr></tbody></table>

1.  客户端 API

    1.  操作文档：

        1.  组成部分：

            1.  Window 载入浏览器的标签

            2.  Navigator 浏览器存在于 Web 上的状态和标识（用户代理）

            3.  Document 载入窗口的实际页面

        <!-- -->

        1.  基本的 DOM 操作

            1.  document -&gt; querySelector querySelectorsAll

            2.  document -&gt; getElementById getElementsByClassName getElementsByTagName

        <!-- -->

        1.  创建并放置新的节点

            1.  document.createElement()

            2.  element.appendChild()

            3.  document.createTextNode()

        <!-- -->

        1.  移动和删除元素

            1.  document.removeChild()

        <!-- -->

        1.  操作样式和属性：

            1.  Document.stylesheets -&gt; CSSStyleSheet\[\]

            2.  element.style.property = value

            3.  element -&gt; textContext href/src/…

            4.  element.setAttribute('property', 'value')

        <!-- -->

        1.  从 Window 对象获取有用的信息

            1.  window -&gt; innerWidth innerHeight

            2.  element -&gt; getBoundingClientRect() -&gt; left/top

    <!-- -->

    1.  从服务器获取数据

        1.  Ajax Asynchronous JavaScript and XML

            1.  const request = new XMLHttpRequest(); request.open('GET', url); request.responseType = 'text'; request.onload = () =&gt; { cb(request.response); }; request.send()

            2.  fetch(url).then(response =&gt; { response.text().then(text=&gt; { cb(text); }) })

    <!-- -->

    1.  绘图

        1.  canvas 见 &lt;canvas-paint&gt; 项目

        2.  动画：window.requestAnimationFrame() 和 window.cancelAnimationFrame()

        3.  webgl 见 &lt;canvas-paint&gt; 项目

    <!-- -->

    1.  视频和音频 API

        1.  HTMLMediaElement

            1.  media -&gt; play() / pause()

            2.  media -&gt; currentTime = number;

    <!-- -->

    1.  客户端存储：

        1.  cookies

            1.  allCookies = document.cookie; document.cookie = newCookie;

            2.  = 被重载，实际是追加

            3.  可以绕过路径限制，比如创建一个指向限制路径隐藏的 iframe，然后访问 contentDocument.cookie，只有使用其它子域名并利用同源策略才能保护

            4.  大小 4KB

        <!-- -->

        1.  Web Storage API

            1.  用法：

                1.  localStorage -&gt; setItem() / getItem() / removeItem() / clear()

                2.  sessionStorage -&gt; setItem() / getItem() / removeItem() / clear()

            <!-- -->

            1.  限制：

                1.  每个域名都有单独的数据存储区

                2.  若禁用第三方 cookie，则不允许第三方 Iframes 对 Web Storage 访问

                3.  localStorage 重启后存在，sessionStorage 重启后不存在

                4.  浏览器崩溃重启后，每个源的 localStorage 限制在 10M

        <!-- -->

        1.  IndexedDB API

            1.  使用：

                1.  let db; const request = window.indexedDB.open('notes', 1); request.onerror = errcb; request.onsuccess = () =&gt; { db = request.result; } request.onupgradeneeded = (e) =&gt; { let db = e.target.result; let objectRestore = db.createObjectStore('notes', { keyPath: 'id', autoIncrement: true }); objectRestore.createIndex('title', 'title', {unique: false}); };

                2.  let transaction = db.transaction(\['note'\], 'readwrite'); let objectStore = transaction.objectStore('notes'); const request = objectStore.add(newItem); request.onsuccess = sucCb; transaction.oncomplete = compCb; transaction.onerror = errCb;

                3.  let objectStore = db.transaction('notes').objectStore('notes'); objectStore.openCursor().onsuccess = e =&gt; { let cursor = e.target.result; if(cursor) { do(cursor.value\['objectStoreKey'\]); cursor.continue(); } else { } };

                4.  let request = objectStore.delete(noteId); transaction.oncomplete()

        <!-- -->

        1.  Cache API

            1.  if('serviceWorker' in navigator) { navigator.serviceWorker.register('relativeurl').then(cb); self.addEventListener('install', e=&gt;{ e.waitUtil(caches.open('video-store').then((cache)=&gt;{ return cache.addAll(\[…\]) })) }) }

> self.addEventListener('fetch', function(e) {
>
> console.log(e.request.url);
>
> e.respondWith(
>
> caches.match(e.request).then(function(response) {
>
> return response || fetch(e.request);
>
> })
>
> );
>
> });
>
>  

1.  其它：

    1.  计时器：setInterval 方法可以使用 clearInterval 方法取消

    2.  取消提交：e.preventDefault()

<!-- -->

1.  对象系统

    1.  继承和原型链：

        1.  基于原型链的继承

            1.  someObject.\[\[Prototype\]\] 指向 someObject 的原型，它可以通过 Object.getPrototypeOf() 和 Object.setPrototypeOf() 访问器访问，等同于 JavaScript的非标准属性 \_\_proto\_\_

            2.  被构造函数创建的实例对象的 \[\[prototype\]\] 指向构造函数的 prototype 属性

        <!-- -->

        1.  继承方法

            1.  函数的继承和覆盖是通过原型链与基于原型链的变量遮蔽实现的

            2.  Object.create(obj) 可以创造一个继承自 o 的对象

    <!-- -->

    1.  使用不同方法创建对象和生成原型链：

        1.  基于语法结构创建的对象

            1.  const o = {a:1} // 继承对象

            2.  const a = \["yo", "whadup", "?"\] // 继承 Array 和 object

            3.  function f() { return 2; } // 继承 Function 和 object

        <!-- -->

        1.  使用构造器创造的对象

            1.  new 和 Function 组合，g.\[\[Prototype\]\] 指向 Cons.prototype

        <!-- -->

        1.  使用 Object.create() 创建的对象

            1.  b = Object.create(a) // b.\[\[Prototype\]\] 指向 a

            2.  Object.create(null) 可以创建没有原型的对象

        <!-- -->

        1.  使用 class 关键字创建的对象

            1.  语法糖，基于原型

            2.  关键字 class constructor static extends super

    <!-- -->

    1.  四种拓展原型链的方法

        1.  ConsFunc.prototype.property = value

        2.  const proto = Object.create(a.prototype); EmptyBConsFunc.prototype = proto;

        3.  Object.setPrototypeOf(proto, a.prototype); EmptyBConsFunc.prototype = proto;

        4.  不建议使用 \_\_proto\_\_

    <!-- -->

    1.  性能：

        1.  要遍历自己和原型链上的所有可枚举方法，使用 for in

        2.  要遍历自己的所有属性，使用 Object.getOwnPropertyNames

        3.  要遍历自己的所有可枚举属性，使用 Object.keys

    <!-- -->

    1.  实例：

        1.  创建对象的两种方式：原型模式和字面量模式

            1.  方法一：

> function createModuleWithLiterals(str1, str2) {
>
>     return {
>
>         greeting: str1, 
>
>         name: str2, 
>
>         sayIt: function() { return \`${this.greeting}, ${this.name}\`;  };
>
>     };
>
> }

1.  方法二：

> function createModuleWithPrototype(str1, str2) {
>
> function Obj() {
>
> this.greeting = str1;
>
> this.name = str2;
>
> }
>
> Obj.prototype.sayIt = function() { return \`${this.greeting}, ${this.name}\`; };
>
> return new Obj();
>
> }

1.  严格模式：

    1.  作用：

        1.  严格模式通过抛出错误消除了一些原有的静默行为

            1.  不允许不声明的赋值导致的全局名称，而报出 ReferenceError

            2.  给不可写属性和不可拓展对象赋值不会引起静默失败，而是抛出 TypeError 异常

            3.  删除不可删除属性时，不会引起静默失败，而是抛出 TypeError 异常

            4.  严格模式下重名参数会被认为是语法错误，不会静默失败

            5.  严格模式禁止八进制语法

            6.  严格模式禁止设置primitive值的属性，不会静默失败，而会抛出 TypeError 异常

        <!-- -->

        1.  修复了一些 JS 引擎难以执行优化的缺陷

            1.  禁用 with，因为运行时才能确定 with 内的变量指向

            2.  eval 不再为上层范围引入新变量，严格模式中的 eval 会被当做执行的字符串中开启严格模式

            3.  严格模式禁止删除不可设置变量（不可设置属性和var let const 等创建的变量）

            4.  eval 和 arguments 不能通过程序语法被绑定和赋值

            5.  arguments 会保留调用时的原始参数

            6.  不再支持 arguments.callee, 它作用很小且不利于优化

            7.  被通过 this 传递给函数的值不会强制转换为对象（正常模式下，传 null 和 undefined 会被转换为全局对象，其它基本类型会被转换成对象）

            8.  argumenst.caller 在严格模式下不可删除不可赋值取址

            9.  禁用 fun.caller 和 fun.arguments

        <!-- -->

        1.  禁用了 EMCAScript 未来版本中可能会定义的一些语法

            1.  禁止了很多关键字的使用

            2.  禁止了不再脚本或者函数层面上的函数声明

    <!-- -->

    1.  调用严格模式：

        1.  为脚本开启：

            1.  "use strict";

            2.  合并严格模式和非严格模式的代码时，请一个个函数去开启

        <!-- -->

        1.  为函数开启：

            1.  function strict() { "use strict"; }

<!-- -->

1.  类型化数组

    1.  ArrayBuffer 表示一个通用的固定长度的二进制数据缓冲区，不能直接操作其中的内容，而要创建一个类型化数组视图或者一个描述缓冲数据格式的 DataView

    2.  类型数组视图：

        1.  构造函数：（buffer\[, byteOffset \[, length\]\])

<table><thead><tr class="header"><th><strong>Type</strong></th><th><strong>Value Range</strong></th><th><strong>Size in bytes</strong></th><th><strong>Description</strong></th><th><strong>Web IDL type</strong></th><th><strong>Equivalent C type</strong></th></tr></thead><tbody><tr class="odd"><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Int8Array"><span class="underline">Int8Array</span></a></td><td>-128 to 127</td><td>1</td><td>8-bit two's complement signed integer</td><td>byte</td><td>int8_t</td></tr><tr class="even"><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8Array"><span class="underline">Uint8Array</span></a></td><td>0 to 255</td><td>1</td><td>8-bit unsigned integer</td><td>octet</td><td>uint8_t</td></tr><tr class="odd"><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint8ClampedArray"><span class="underline">Uint8ClampedArray</span></a></td><td>0 to 255</td><td>1</td><td>8-bit unsigned integer (clamped)</td><td>octet</td><td>uint8_t</td></tr><tr class="even"><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Int16Array"><span class="underline">Int16Array</span></a></td><td>-32768 to 32767</td><td>2</td><td>16-bit two's complement signed integer</td><td>short</td><td>int16_t</td></tr><tr class="odd"><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint16Array"><span class="underline">Uint16Array</span></a></td><td>0 to 65535</td><td>2</td><td>16-bit unsigned integer</td><td>unsigned short</td><td>uint16_t</td></tr><tr class="even"><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Int32Array"><span class="underline">Int32Array</span></a></td><td>-2147483648 to 2147483647</td><td>4</td><td>32-bit two's complement signed integer</td><td>long</td><td>int32_t</td></tr><tr class="odd"><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Uint32Array"><span class="underline">Uint32Array</span></a></td><td>0 to 4294967295</td><td>4</td><td>32-bit unsigned integer</td><td>unsigned long</td><td>uint32_t</td></tr><tr class="even"><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Float32Array"><span class="underline">Float32Array</span></a></td><td>1.2×10<sup>-38</sup> to 3.4×10<sup>38</sup></td><td>4</td><td>32-bit IEEE floating point number (7 significant digits e.g., 1.1234567)</td><td>unrestricted float</td><td>float</td></tr><tr class="odd"><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Float64Array"><span class="underline">Float64Array</span></a></td><td>5.0×10<sup>-324</sup> to 1.8×10<sup>308</sup></td><td>8</td><td>64-bit IEEE floating point number (16 significant digits e.g., 1.123...15)</td><td>unrestricted double</td><td>double</td></tr><tr class="even"><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt64Array"><span class="underline">BigInt64Array</span></a></td><td>-2<sup>63</sup> to 2<sup>63</sup>-1</td><td>8</td><td>64-bit two's complement signed integer</td><td>bigint</td><td>int64_t (signed long long)</td></tr><tr class="odd"><td><a href="https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigUint64Array"><span class="underline">BigUint64Array</span></a></td><td>0 to 2<sup>64</sup>-1</td><td>8</td><td>64-bit unsigned integer</td><td>bigint</td><td>uint64_t (unsigned long long)</td></tr></tbody></table>

1.  数据视图：

    1.  DataView 是一种底层接口，它提供有可以操作缓冲区中任意数据的读写接口，可以改变大端字节序和小端字节序

<!-- -->

1.  使用类型数组的 Web API：

    1.  FileReader.prototype.readAsArrayBuffer()

    2.  XMLHttpRequest.prototype.send()

    3.  ImageData.data

<!-- -->

1.  转换为普通数组

    1.  Array.from()

> var typedArray = new Uint8Array(\[1, 2, 3, 4\]),
>
> normalArray = Array.prototype.slice.call(typedArray);
>
> normalArray.length === 4;
>
> normalArray.constructor === Array;
>
>  
