# Event Loop

Since it is the only way to return to the outer frame that finish all our task, we should avoid to be blocked here. V8 use event loop to be asynchronous in a single master thread.

In V8 event loop, it sees `script code`、`setTimeout`、`setInterval`、`setImmediate` (In node)、`I/O`、`UI Rendering` (Browser) as macro tasks, or tasks; And `Process.nextTick` (Node)、`Promise`、`MutationObserver` (Browser) as micro tasks. After a macro task run, all micro tasks in the micro tasks queue must be executed.

Pseudo-code is in the following:

```javascript

const tasks = new Queue();
const microTasks = new Queue();

function runAllMicroTasks() {
  while(microTasks.length>=0) {
    const microTask = microTasks.dequeue();
    microTasks.run(microTasks,tasks);
  }
}

tasks.enqueue(script_code);
while(tasks.length>0) {
  const task = microTasks.dequeue();
  task.run(microTasks,tasks);
  runAllMicroTasks();
}
runAllMicroTasks();
```

For example, for the following script

```javascript
console.log('script start');
setTimeout(function() {
  console.log('setTimeout');
}, 0);
Promise.resolve().then(function() {
  console.log('promise1');
}).then(function() {
  console.log('promise2');
});
console.log('script end');
```

**Start**

```
- Tasks: [script code]
- Microtasks: []
- Current frame: null
- Log: []
```

**Execution 1 end**

Run script code:

```
- Tasks: [setTimeout-callback]
- Microtasks: [promise1 then]
- Current frame: script code
- Log: [script start, script end]
```

**Execution 2**

Run promise1:

```
- Tasks: [setTimeout callback]
- Microtasks: [promise2 then]
- Current frame: promise1 then
- Log: [script start, script end, promise1]
```

Run promise2:

```
- Tasks: [setTimeout callback]
- Microtasks: []
- Current frame: promise2 then
- Log: [script start, script end, promise1, promise2]
```

**Execution 3 end**

Run setTimeout:

```
- Tasks: []
- Microtasks: []
- Current frame: setTimeout callback
- Log: [script start, script end, promise1, promise2, setTimeout]
```