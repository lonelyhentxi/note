# React Kernel Simulation

[Reference](https://github.com/jackiewillen/build-your-own-react)

## Version 1: Create element with tagname and its child's text content

App:

```ts
const helloWorld = React.createElement('div', null, `Hello World`);
ReactDOM.render(helloWorld, document.getElementById('root'));
```

Lib:

```ts
function createElement(parentEl: string, props: any, childEl: string) {
    const parentElement = document.createElement(parentEl);
    parentElement.innerHTML = childEl;
    return parentElement;
}

function render(insertEl: ElementRef, rootEl: ElementRef) {
    rootEl.appendChild(insertEl);
}

React = {
    createElement
}

ReactDOM = {
    render
}
```

## Version 2: Add the function to create element with a function

App:

```ts
const Hello = function () {
return React.createElement('div', null, `Hello Version2.0`);
};
const helloWorld = React.createElement(Hello, null, null);
ReactDOM.render(helloWorld, document.getElementById('root'));
```

Lib:

```ts
function createElement(parentEl: string | () => ElementRef,
                       props: any, childEl: string | ElementRef ) {
    if (typeof parentEl === 'function') {
        return parentEl();
    }
    else {
        const parentElement = document.createElement(parentEl);
        if(typeof childEl === 'string') {
            parentElement.innerHTML += childEl;
        } else if(typeof childEl === 'object') {
            parentElement.appendChild(childEl);
        }
        return parentElement;
    }
}

function render(insertEl: ElementRef, rootEl: ElementRef) {
    rootEl.appendChild(insertEl);
}

React = {
    createElement
}

ReactDOM = {
    render
}
```

## Version 3: Add the function that can have more children

App:

```ts
const HelloVersion3 = function () {
return React.createElement('div', null, `Version 3.0`);
};
const helloWorld1 = React.createElement(HelloVersion3, null, null);
const helloWorld2 = React.createElement(HelloVersion3, null, null);
const divEle = React.createElement('div', null, `wrapped by a div`);

const parent = React.createElement('div', null,
        helloWorld1,
        helloWorld2,
        divEle,
        `text context`
);

ReactDOM.render(parent, document.getElementById('root'));
```

Lib:

```ts
function createElement(parentEl: string | () => ElementRef,
                       props: any, ...childEls: (string | ElementRef)[] ) {
    if (typeof parentEl === 'function') {
        return parentEl();
    }
    else {
        const parentElement = document.createElement(parentEl);
        childEles.forEach(child => {
            if(typeof child === 'string') {
                parentElement.innerHTML += child;
            }
            else if (typeof child === 'object') {
                parentElement.appendChild(child);
            }
        })
        return parentElement;
    }
}

function render(insertEl: ElementRef, rootEl: ElementRef) {
    rootEl.appendChild(insertEl);
}

React = {
    createElement
}

ReactDOM = {
    render
}
```

## Version 4: Support props

App:

```ts
const Hello = ({name}) => {
	return React.createElement('div', null, `This is ${name}`);
};

const helloWorld = React.createElement(Hello, {name: 'version 5'}, null);
ReactDOM.render(helloWorld, document.getElementById('root'));
```

Lib:

```ts
function createElement(parentEle: string | (any) => ELementRef, props: any,
                       ...childEles: (string | ElementRef)[]) {
    if (typeof parentEle === 'function'){
        return parentEle(props);
    } else {
        let parentElement = document.createElement(parentEle);
        childEles.forEach(child => {
            if(typeof child === 'string') {
                parentElement.innerHTML += child;
            } else if(typeof child === 'object') {
                parentElement.appendChild(child);
            }
        });
        return parentElement;
    }
}
function render(insertEle, rootEle) {
    rootEle.appendChild(insertEle);
}
React = {
    createElement
}
ReactDOM = {
    render
}
```

## Version 5: Support components

App:

```ts
class Hello extends React.Component {
    constructor(props) {
        super(props);
    }
    render() {
        return React.createElement('div', null, `Hello ${this.props.name}`);
    }
}
const helloWorld = React.createElement(Hello, {name: '文字'}, null);
ReactDOM.render(helloWorld, document.getElementById('root'));
```

For the lib, we must can know whether it is function or component.

- **Why can not use `new` at all?**: Lambda `=>` does not has its own `this`, so we can not use `new`
- **Why can not not use `new` at all?**:
    - If we don't use `new`, it will throw a `TypeError` when it's a class. 
    - The `new` operator may change the object: If a function return nothing or not a object, you will get the `this` object, a instance of the class, having same name of the function. ELse you will get the object as the return value.

So we should set a flag, but **why we can not just extends the `React.Component` and just `instanceof`?** There may be two or more copies of `React.Component`, making it have risks. **Maybe we can just detect the `render` function?** Sure, but the designers didn't have the idea that if the API would be changed then.

At the end, the designers add a prototype variable `isReactComponent` to the `React.Component` class



```ts
class Component {
    constructor(props) {
        this.props = props;
    }
}

// In react, there is {} instead of true,
// because of a bug of the old version of Jest
Component.prototype.isReactComponent = true; 

function createElement(parentEle: Component | (any) => ElementRef, 
                       props: any, ...childEles: (ElementRef | string)[]) {
    if (parentEle.isReactComponent) {
        let component = new parentEle(props);
        return component.render();
    } else if (typeof parentEle === 'function') {
        return parentEle(props);
    } else {
        let parentElement = document.createElement(parentEle);
        childEles.forEach(child => {
            if(typeof child === 'string') {
                parentElement.innerHTML += child;
            } else if(typeof child === 'object') {
                parentElement.appendChild(child);
            }
        });
        return parentElement;
    }
}
function render(insertEle, rootEle) {
    rootEle.appendChild(insertEle);
}
React = {
    createElement,
    Component
}
ReactDOM = {
    render
}
```

## Version 6: Support clicking

App:

```ts
class MyButton extends React.Component {
constructor(props) {
  super(props);
}
render() {
  return React.createElement('button', {onclick: this.props.onClick}, `Click me`);
}
}
const myBtn = React.createElement(MyButton, {onClick: () => alert('click')}, null);
ReactDOM.render(myBtn, document.getElementById('root'));
```

Lib:

```ts
class Component {
    constructor(props) {
        this.props = props;
    }
}

Component.prototype.isReactComponent = true; 

function createElement(parentEle: Component | (any) => ElementRef, 
                       props: any, ...childEles: (ElementRef | string)[]) {
    if (parentEle.isReactComponent) {
        let component = new parentEle(props);
        return component.render();
    } else if (typeof parentEle === 'function') {
        return parentEle(props);
    } else {
        let parentElement = document.createElement(parentEle);
        Object.keys(props).forEach(key => {
            switch(key) {
                case 'onclick':
                    parentElement.addEventListener('click', props[key]);
                    break;
                default:
                    break;
            }
        });
        childEles.forEach(child => {
            if(typeof child === 'string') {
                parentElement.innerHTML += child;
            } else if(typeof child === 'object') {
                parentElement.appendChild(child);
            }
        });
        return parentElement;
    }
}
function render(insertEle, rootEle) {
    rootEle.appendChild(insertEle);
}
React = {
    createElement,
    Component
}
ReactDOM = {
    render
}
```

## Version 7: Reactive

App:

```ts
class Counter extends React.Component {
    constructor(props) {
      super(props);
      this.state = {value: 0};
    }
    onPlusClick() {
      this.setState({value: this.state.value + 1});
    }
    onMinusClick() {
      this.setState({value: this.state.value - 1});
    }
    render() {
      return React.createElement('div', null,
        React.createElement('div', null, `The Famous Dan Abramov's Counter`),
        React.createElement('div', null, `${this.state.value}`),
        React.createElement('button', {onClick: this.onPlusClick.bind(this)}, '+'),
        React.createElement('button', {onClick: this.onMinusClick.bind(this)}, '-')
      );
    }
}
let myCounter = React.createElement(Counter,null,null);
ReactDOM.render(myCounter, document.getElementById('root'));
```

Lib:

```ts
let rootElement, rootReactElement;

class Component {
    constructor(props) {
        this.props = props;
    }
    setState(state) {
        this.state = state;
        reRender();
    }
}

Component.prototype.isReactComponent = true; 

function createElement(parentEle: Component | (any) => ElementRef, 
                       props: any, ...childEles: (ElementRef | string)[]) {
    if (parentEle.isReactComponent) {
        let component = new parentEle(props);
        return component;
    } else if (typeof parentEle === 'function') {
        return parentEle(props);
    } else {
        let parentElement = document.createElement(parentEle);
        Object.keys(props || {}).forEach(key => {
            switch(key) {
                case 'onclick':
                    parentElement.addEventListener('click', props[key]);
                    break;
                default:
                    break;
            }
        });
        childEles.forEach(child => {
            if(typeof child === 'string') {
                parentElement.innerHTML += child;
            } else if(typeof child === 'object') {
                parentElement.appendChild(child);
            }
        });
        return parentElement;
    }
}
function render(insertEle: Component, rootEle: ElementRef) {
    rootElement = rootEle;
    rootReactElement = insertEle;
    rootEle.appendChild(insertEle.render());
}

function reRender() {
    while(rootElement.hasChildNodes()) {
        rootElement.removeChild(rootElement.lastChild);
    }
    ReactDOM.render(rootReactElement, rootElement);
}

React = {
    createElement,
    Component
}

ReactDOM = {
    render
}
```