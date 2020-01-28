# Vue Kernel Simulation

[Reference here](https://juejin.im/post/5df2e2d96fb9a0165e330af9). Learn, note and add type hints.

The dep:

```ts
let watcher = null;
class Dep {
    this.subs: Watch[] = [];
    constructor() {}
    // notify all sub watchers
    notify() {
        this.subs.forEach(sub => sub.update());
    }
    addSub(watch: Watch) {
        // add the watch which related to the variable
        this.subs.push(watch);
    }
}
```

The watch class implement:

```ts
class Watch {
    vue: Vue;
    exp: Expression;
    cb: (Vue, any) => void;
    hasAddedAsSub: boolean;

    constructor(vue: Vue, exp: Expression, cb: (Vue, any)=>void) {
        this.vue = vue;
        this.exp = exp;
        this.cb = cb;
        this.hasAddedAsSub = false; // avoid to add duplicated sub watch
        this.value = this.get(); // first get the value
    }

    get(): any {
        watcher = this;
        const value = this.vue[this.exp];
        Watcher = null;
        return value;
    }

    update() {
        const value = this.get();
        const oldValue = this.value;
        if(value !== oldValue) {
            this.value = value;
            this.cb.call(this.vue, value); 
            // use new value to call callback, such as update the page view
        }
    }
}
```

```ts
class Observer {
    constructor(data: object) {
        // hijack the user-defined vue data
        // to observe all elements of it, implementing bi-binding
        this.defineReactive(data);
    }
    defineReactive(data: object) {
        const dep = new Dep();
        Object.keys(data).forEach(key => {
            const val = data[key];
            Object.defineProperty(data, key, {
                get() {
                    if(Watcher) {
                        if(!watch.hasAddedAsSub) {
                            dep.addSub(Watcher);
                            Watcher.hasAddedAsSub = true;
                        }
                    }
                    return val;
                }
                set(newVal) {
                    if(newVal === val) {
                        return;
                    }
                    val = newVal;
                    dep.notify();
                }
            })
        })
    }
}
```

Compile:

```ts
class Compile {
    $vue: Vue;
    $el: ElementRef;

    constructor(el: DOMString, vue: Vue) {
        this.$vue = vue;
        this.$el = document.querySelector(el);
        if(this.$el) {
            const $fragment = this.nodeToFragment(this.$el);
            this.comileText($fragment.childNodes[0]);
            this.$el.append($fragment);
        }
    }

    nodeToFragment(el: ElementRef) {
        const fragment = document.createDocumentFragment();
        fragment.appendChild(el.firstChild);
        return fragment;
    }

    compileText(node: Node) {
        const reg = /\{\{(.*)\}\}/;
        if(reg.test(node.textContent)) {
            const matchedName = RegExp.$1;
            node.textContent = this.$vue[matchedName];
            new Watch(this.$vue, matchedName, function(value) {
                node.textContext = value;
            });
        }
    }
}
```

Vue:

```ts
class Vue {
    _data : object || undefined;

    constructor(options: { data: object? , el: DOMString? }) {
        let data = this._data = options.data || undfined; 
        this._initData();
        new Observer(data);
        new Compile(options.el, this);
    }

    _initData() {
        // bind the elements of options data to the vue object
        const that = this;
        Object.keys(that._data).forEach((key) => {
            Object.defineProperty(that, key, {
                get: () => {
                    return that._data[key];
                }, 
                set: (newVal) => {
                    that._data[key] = newVal;
                }
            })
        })
    }

    
}
```