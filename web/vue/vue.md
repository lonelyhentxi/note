## 深入响应式原理

vue最显著的特性之一便是不太引人注意的响应式系统（reactivity system）。其模型层只是普通的Javascript对象，修改之则更新视图（view）。

---

### 如何追踪变化

把一个普通的Javascript对象传给vue实例的data选项，vue会遍历此对象的所有属性，使用Object.defineProperty把这些属性全部转化为getter和setter。这意味无法在ES5之前使用，且无法shim。

用户看不到vue在内部实现的vue的追踪依赖（在属性被访问和修改时通知变化），浏览器打印对象时对其格式化并不同。（使用vue-devtools可以执行友好的检查接口）

每个组件都有相应的watcher实例对象，会在渲染中将对应的属性记录为依赖。当相应的setter被调用时，会通知这个watch重新计算，更新关联组件。

---

### 变化检测问题

由于Vue在初始化实例时执行字段到属性的转换，所以必须在data对象存在才能转换，因而才能是响应的。

```js
var vm = new Vue({
    data:{
        a:1
    }
});

vm.b = 2;

//a是响应的，b是非响应的
```

由于是在初始化期间转换，vue不支持向已经创建的实例上动态添加新的根级响应式属性（root-level reactive property）。可以用`Vue.set(object,key,value)`的方式将响应的属性添加到嵌套的对象上`Vue.set(vm.someObject,'b',2)`；或者使用`vm.$set(this.someObject,'b',2)`；或者创建一个包含原对象的属性和新的属性的新的对象：
```javascript
this.someObject = Object.assign({},this.someObject,{a:1,b:2});
```

---

### 声明响应式属性

由于vue不允许动态添加根级响应式属性，所以必须在初始化实例前声明根级响应式属性。

这样的限制消除了在依赖项跟踪系统中的一类边界情况，也使得Vue实例在类型检查系统的帮助下更加有效的运行。也便于后续代码被理解。

---

### 异步更新队列

vue异步执行DOM更新，只要观察到数据变化，Vue将开启一个队列，并且缓冲在同一个事件循环中发生的数据改变。如果一个watch被多次触发，只会一次推入队列当中。在下一次循环当中，Vue刷新队列并执行实际（已经去重的）工作。

Vue对异步队列使用原生的`Promise.then`和`MutationObserver`如果环境不支持，会使用原生的`setTimeout(n,0)`来代替。

组件会在事件循环队列清空的下一个队列更新，如果要在数据变化之后等待完成更新DOM可以使用`Vue.nextTick(callback)`,这样回调函数在DOM完成更新后就会直接调用。

```html
<div id='example'>{{message}}</div>
```

```js
var vm=new Vue({
    el:'example',
    data:{
        message:'123'
    }
});
vm.message='new message';//更新数据
vm.$el.textContent=='new message';//错误的用法
Vue.nextTick(function(){
    vm.$el.textContent=='new message';//正确的用法
})
```

在组件内部使用`vm.$nextTick()`实例方法，非常方便，不需要全局Vue，并且回调函数中的`this`可以自动绑定到当前实例上。

```js
Vue.component('example',{
    template:'<span>{{message}}</span>',
    data：function(){
        return{
            message:'没有更新'
            }
    },
    methods:{
        updateMessage:function(){
            this.message='更新完成';
            console.log(this.$el.textContent);//=>没有更新
            this.$nextTick(function(){
                console.log(this.$el.textContext);
            })
        }
    }
});
```

---
---

## 过渡效果

---

### 概述

Vue在插入、更新或者移除DOM效果时，提供多种不同方式的应用过渡效果：

- 在css过渡和动画中自动应用class
- 配合使用第三方css动画库，如animate.css
- 在过渡钩子函数中使用javascript直接操作DOM
- 配合使用第三方Javascript动画库，如Velocity

---

### 单组件/元素的过渡

Vue提供了transition的封装组件，在下列情形中，可以给任何组件添加entering、leaving的过渡：

- 条件渲染：使用v-if
- 条件展示：使用v-show
- 动态组件
- 组件根节点

一个典型的例子
```html
<div id='demo'>
    <bitton @click='show=!show'>
    Toggle
    </button>
    <transition name='fade'>
        <p v-if='show'>hello</p>
    </transition>
</div>
```

```js
new Vue({
    el:"#demo",
    data:{
        show:true
    }
});
```

```css
.fade-enter-active, .fade-leave-active{
    tranistion: opacity .5s
}

.fade-enter, .fade-leave-to {
    opacity:0
}
```
当插入或者删除包含在`transition`组件中的元素时，Vue会：
1. 自动嗅探元素是否应用了css过渡或者动画，如果是，在恰当的时机添加或者删除css类名称。
2. 如果过渡组件提供了javascript钩子函数，这些钩子函数将会在恰当的事件被调用。
3. 如果没有找到钩子函数也没有检测到css过渡/动画，DOM操作在下一帧中立即执行。（这是指浏览器逐帧动画，与Vue的nextTick概念不同）

**过渡的-css-类名**

会有6个css类名在enter和leave过渡中切换：
1. v-enter：定义进入过渡的开始阶段，在元素被插入时生效，在下一帧被移除
2. v-enter-active：定义过渡的状态，在元素的整个过渡过程中作用，在元素被插入时生效，在transition、animation完成后被移除；这个类可以被用来定义过渡的过程时间，延迟和曲线函数。
3. v-enter-to：在元素被插入一帧后生效（此时v-enter被移除），在transition、animation完成以后删除。
4. v-leave:定义离开过渡的开始状态，在离开过渡被触发时生效，下一帧移除。
5. v-leave-active:定义过渡的状态，在整个过渡中作用，在离开过渡被触发后立即生效，在transition/animation完成之后移除。这个类可以用来定义过渡的过程时间，延迟和曲线函数。
6. v-leave-to：定义离开过渡的结束状态。在离开过渡被触发后一帧生效，通识v-leave被删除），在transition、animation完成之后删除。

**css过渡**

练习的例子：
```html
<div id='example-1'>
    <button @click='show=!show'>
    Toggle Button
    </button>
    <transition name='slide-fade'>
     <p v-if='show'>hello</p>
    </transition>
</div>
```

```javascript
new Vue({
    el:'#example-1',
    data:{
        show:false
    }
})
```

```css
.slide-fade-enter-active{
    transition:all .3s ease;
}
.slide-fade-leave-active{
    transition:all .8s cubic-bezier{1.0,0.5,0.8,1.0}
}
.slide-fade-enter,.slide-fade-leave-to{
    transform:translateX(10px);
    opacity:0;
}
```

**css动画**

css动画的用法同css过渡，区别是动画v-enter类名在节点插入Dom后不会立即被删除，而是杂animationend事件被触发时删除。

```html
<div id='example-2'>
    <button @click='show=!show'>Toggle</button>
    <transition name='bounce'>
        <p v-if='show'>Look at me!</p>
    </transition>
</div>
```

```javascript
new Vue({
    el:'#example',
    data:{
        show:true
    }
})
```

```css
.bounce-enter-active{
    animation:bounce-in .5s
}
.bounce-leave-active{
    animation:bounce-in .5s reverse
}
@keyframes bounce-in{
    0% {
        transition:scale(0);
    }
    50%{
        transition:scale(1.5);
    }
    100%{
        transition:scale(1);
    }
}
```

**自定义过渡类名**

可以通过以下特性来自定义过渡类名：

- enter-class
- enter-active-class
- enter-to-class

……

这些类的优先级高于普通的类名，对于Vue的过渡系统和其它第三方css动画库，例如Animate.css结合使用十分有用。

```html
<link href='https://unpkg.com/animate.css@3.5.1/animate.min.css' rel='stylesheet' type='text/css'>

<div id='example-3'>
    <button @click='show!=show'>
    Toggle
    </button>
    <transition
        name='custom-classes-transition'
        enter-active-class='animated tada'
        leave-active-class='animated bounceOutRight'
        >
        <p v-if='show'>hello</p>
    </transition>
</div>
```
```javascript
new Vue({
    el:'#example-3',
    data:{
        show:true
    }
});
```

**同时使用transitions和animations**

Vue为了知道过渡的完成，必须设置相应的事件监听器，可以是transitionend或者animationend，取决于给元素运用的css规则。如果使用其中一种，Vue能够自动识别并且监听。

当同时设置两种过渡特效的是时候，animation很快触发被完成了，而transition的效果还没有结束。在这样的情况下，需要使用type特性并且设置animation和transition来明确声明需要Vue监听的类型。

**声明过渡持久性**

在大多数情况下，Vue可以自动的计算出过渡已经被完成了。Vue将会等待 transitionend和animationend事件（根过渡元素上的）其中最先完成的那个。然而，这并不总是按照我们意料之中发展，例如：我们可能有一个编排过渡序列，在内部的过渡元素的的持续时间比外部的长。

在这种情况下，我们需要显式的过渡持续时间声明，使用transition标签的duration属性：

```html
<transition :duration='1000'>...</transition>
```
你也可以为进入和离开事件分别制定一个值

```html
<transition :duration="{enter:500,leave:800}">..</transition>
```

**Javascript钩子**

可以在属性中声明Javascript钩子

```html
<transition 
    v-on:before-enter='beforeEnter'
    v-on:enter="enter"
    v-on:after-enter="afterEnter"
    ...
>
..
</transition>
```

```js
//..
methods:{
    //..
    //进入中
    //..
    beforeEnter:function(el){
        //..       
    },
    //此回调函数是可选项的设置
    //与css结合时候使用
    enter:function(el,done){
        //..
        done()
    },
    //..
}
```
这些钩子可以结合css 的transitions/animations使用，也可以单独使用

AT：当只使用Javascript过渡的时候，在enter和leave中，回调函数done是必须的，否则他们会被同步调用，过渡会立即完成

一个使用Velocity.js的简单例子
```html
<script src='https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js'></script>

<div id='example-4'>
    <button @click='show=!show'>
     Toggle
    </button>
    <transition
        v-on:before-enter='beforeEnter'
        v-on:enter='enter'
        v-on:leave='leave'
        v-bind:css='false'
    >
        <p v-if='show'>
         Demo
        </p>
    </transition>
</div>
```

```js
new Vue({
    el:'#example-4',
    data:{
        show:false
    },
    methods:{
        beforeEnter:function(el){
            el.style.opacity=0;
            el.style.transformOrigin='left';
        },
        enter:function(el,done){
            Velocity(el,{opacity:1,fontSize:'1.4em'},{duration:300});
            Velocity(el,{fontSize:'1em'},{complete:done});
        },
        leave:function(el,done){
            Velocity(el,{tanslateX:'15px',rotateZ:'50deg'},{duration:600});
            Velocity(el,{rotateZ:'100deg'},{loop:2});
            Velocity(el,{rotateZ:'45deg',translateY:'30px',translateX:'30px',opacity:0},{complete:done})
        }
    }
});
```

---

### 初始渲染的过渡

可以通过`appear`特性设置节点的在初始渲染的过渡

```html
<transition appear
    appear-class='custom-appear-class'
    appear-to-class='custom-appear-to-class'
    appear-active-class='custom-appear-active'
>
<!--..-->
</transition>
```

```html
<transition
  appear
  v-on:before-appear="customBeforeAppearHook"
  v-on:appear="customAppearHook"
  v-on:after-appear="customAfterAppearHook"
  v-on:appear-cancelled="customAppearCancelledHook"
>
  <!-- ... -->
</transition>
```

---

### 多个元素的过渡

对于原生标签可以使用v-if，v-else，最常见的多标签过渡是一个列表和描述这个列表为空消息的元素
```html
<transition>
  <table v-if="items.length > 0">
    <!-- ... -->
  </table>
  <p v-else>Sorry, no items found.</p>
</transition>
```

AT：当有相同标签名的元素切换时，需要通过key特性设置唯一的值来标记使得Vue区分它们，否则Vue会为了效率替换相同标签内部的内容。

```html
<transition>
  <button v-if="isEditing" key="save">
    Save
  </button>
  <button v-else key="edit">
    Edit
  </button>
</transition>
```

在一些场景中，也可以给同一个元素的key特性设置不同的状态来代替v-if或者v-else

```html
<transition>
  <button v-bind:key="isEditing">
    {{ isEditing ? 'Save' : 'Edit' }}
  </button>
</transition>
```

使用了v-if的多个元素的过渡可以重写为绑定了单个元素的过渡
```html
<transition>
  <button v-if="docState === 'saved'" key="saved">
    Edit
  </button>
  <button v-if="docState === 'edited'" key="edited">
    Save
  </button>
  <button v-if="docState === 'editing'" key="editing">
    Cancel
  </button>
</transition>
<!--重写为-->
<transition>
  <button v-bind:key="docState">
    {{ buttonMessage }}
  </button>
</transition>
```
```javascript
// ...
computed: {
  buttonMessage: function () {
    switch (this.docState) {
      case 'saved': return 'Edit'
      case 'edited': return 'Save'
      case 'editing': return 'Cancel'
    }
  }
}
```

**过渡模式**

默认下，Vue中的过渡是同时发生的，但是同时生效的离开和进入的过渡不能满足所有的要求，所以Vue提供了过渡模式。

- in-out ：新元素先进行过渡进入，完成之后当前元素过渡离开。
- out-in：当前元素先进行过渡，完成之后新元素过渡进入。

```html
<transition name='fade' mode='out-in'>
    <!--the buttons...-->
</transition>
```

---

### 多个组件的过渡

多个组件的过渡简单很多，不需要使用`key`特性；相反，要使用动态组件。

```html
<transition name='component-fade' mode='out-in'>
    <component v-bind:is='view'></component>
</transition>
```

```js
new Vue({
    el:'#transition-components-demo',
    data:{
        view:'v-a'
    },
    components:{
        'v-a':{
            template:'<div>Component A</div>'
        },
        'v-b':{
            template:'<div>Component B</div>
        }
    }
})
```

```css
.component-fade-enter-active,.component-fade-leave-active {
    transition:opacity .3s ease;
}
.component-fade-enter, .component-fade-leave-active {
    opacity:0;
}
```

---

### 列表过渡

在需要同时渲染整个列表的场景，比如`v-for`中，使用`<transition-group>`组件：

- 不同于`<transition>`，`<transition-group>`会以一个真实的元素呈现，默认呈现方式为一个`<span>`，可以通过其`<tag>`特性将标签换成其他的标签。
- 在这个标签内部的元素总是需要提供唯一的`<key>`属性值。

**列表的进入和离开过渡**

```html
<div id='list-demo' class='demo'>
    <button v-on:click='add'>Add</button>
    <button v-on:click='remove'>Remove</button>
    <transition-group name=list tag='p'>
        <span v-for='item in items' v-bind:key='item' class='list-item'>
        {{item}}
        </span>
    </transition-group>
</div>
```
```js
new Vue({
    el:'#list-demo',
    data:{
        items:[1,2,3,4,5,6,7,8,9],
        nextNum:10
    },
    methods:{
        randomIndex:function(){
            return Math.floor(Math.random()*this.items.length);
        },
        add:function(){
            this.times.splice(this.randomIndex(),0,this.nextNum++);
        },
        remove:function(){
            this.items.splice(this.randomIndex(),1);
        }
    }
});
```
```css
.list-item{
    display:inline-block;
    margin-right:10px;
}
.list-enter-active,.list-leave-active {
    transition:all 1s;
}
.list-enter,.list-leave-to{
    opacity:0;
    transform:translateY(30px);
}
```

**列表的位移过渡**

`<transition-group>`组件还有一个特殊之处，可以使用进入和离开动画，也可以改变定位。`v-move`特性在改变定位的过程中使用。

和之前的类名一样，可以适应`name`属性来自定义前缀，也可以通过`move-class`属性来手动设置：

```html
<script src='https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.14.1/loadsh.min.js' ></script>

<div id='flip-list-demo' class='demo'>
    <button @click='shuffle'>shuffle</button>
    <transition-group name='filp-list' tag='ul'>
        <li v-for='item in items' :key='item'>
        {{item}}
        </li>
    </transition>
</div>
```

```js
new Vue({
    el:'#flip-list-demo',
    data:{
        items:[1,2,3,4,5,6,7,8,9]
    },
    methods:{
        shuffle:function(){
            this.items = _.shuffle(this.items);
        }
    }
})
```

```css
.flip-list-move{
    transition:transform 1s;
}
```

**列表的渐进过渡**

通过data属性和Javascript通信，就可以实现列表的渐进过渡：
```html
<script src='https://cdnjs.cloudflare.com/ajax/libs/velocity/1.2.3/velocity.min.js'></script>

<div id='staggered-list-demo'>
    <input v-model='query'>
    <transition-group
        name='staggered-fade'
        tag='ul'
        v-bind:css='true'
        v-on:before-enter='beforEnter'
        v-on:enter='enter'
        v-on:leave='leave'
    >
        <li
            v-for='(item,index) in computeredList'
            v-bind