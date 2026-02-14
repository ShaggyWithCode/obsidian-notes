- JavaScript is **single-threaded** (used for UI so needed to do one thing at a time on UI to avoid race condition)
- **but** browser/NodeJS (hosting environment) are multi threaded.
- JavaScript runtime environment (browser/NodeJS) has JavaScript engine which has only one call stack which execute one thing at a time.
- as hosting environment can do many things due to many thread, hosting environment gives capability to JavaScript to handle multiple tasks using Event loop.
- so Event Loop is how JavaScript handle multiple things at once. 
```
console.log("Start"); // 1

setTimeout(() => {
  console.log("Timeout"); // 3
}, 0);

Promise.resolve().then(() => {
  console.log("Promise"); // 2
});

console.log("End"); // 1
// Output: Start, End, Promise, Timeout
```

 **How Event Loop Works (Step by Step)**
 ### **1. Call Stack**
 - here function execute - last In , first out (LIFO)
```
function a() { b(); }
function b() { console.log("Hi"); }
a();
// Stack: a() -> b() -> console.log()
```

 ### **2. Web APIs/ NodeJS APIs**
 following async operations comes under web apis and nodejs apis
 - `setTimeout`, `setInterval`
 - Dom events (click, etc.)
 - `fetch()`, `axios()`
 - `fs.readFile()`(NodeJS)
 when such a async operations complete, its callback goes to `Queue` 

 ### **3. Two Types of Queue**

**A. `Microtask` queue**
- its a high priority queue.
- `promises.then()`, `promise.catch()`, `promise.finally()`, 
- `async/await` (uses promise internally)
- `queueMicrotask()`
- `MutstionObserver` (Browser)
- `process.nextTick()` (NodeJS - `run before microtasks`)
**B. `Macrotask` queue**
- `setTimeout` , `setIntervel`
- I/O operation
- UI rendering
- `setImmidiate()` (NodeJS) 
- Event Callbacks (click , etc.)

 ### **3. Event loop algorithm**

1.Execute all synchronous task
2.While call stack empty
	a. check microtask queue
	b. Run all microtask until queue is empty
	c. check microtask queue
	d. run   ONE `macrotask`
	e. Go back to step `a`


**IN UI context**
```
SYNC CODE → MICROTASKS → (RENDER) → MACROTASK → REPEAT
```


- After each macrotask, ALL microtasks must complete before next `macrotask`.
```
console.log("1 - Sync");

setTimeout(() => {
  console.log("2 - Timeout (Macrotask)");
}, 0);

Promise.resolve().then(() => {
  console.log("3 - Promise (Microtask)");
});

console.log("4 - Sync");

// Execution Order:
// 1 - Sync
// 4 - Sync
// 3 - Promise (Microtask) ← ALL microtasks run first
// 2 - Timeout (Macrotask) ← Then ONE macrotask
```

complex example

```
console.log("Start");

setTimeout(() => {
  console.log("Timeout 1");
  Promise.resolve().then(() => {
    console.log("Promise inside Timeout");
  });
}, 0);

setTimeout(() => {
  console.log("Timeout 2");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise 1");
}).then(() => {
  console.log("Promise 2");
});

console.log("End");

/* Output:
Start
End
Promise 1     ← Microtasks run first
Promise 2
Timeout 1     ← First macrotask
Promise inside Timeout ← Microtasks run AFTER EACH macrotask
Timeout 2     ← Second macrotask
*/
```

### **Q1: Why does Promise run before setTimeout?**

**Answer**: Promises go to Microtask Queue, setTimeout goes to Macrotask Queue. Event Loop always processes ALL microtasks before next macrotask.

### **Q2: Can microtasks block the main thread?**

**Answer**: Yes! Microtasks run to completion. If you add microtasks inside microtasks, they'll keep running and block rendering/macrotasks.

```
// DON'T DO THIS - Infinite microtask loop
function infiniteMicrotasks() {
  Promise.resolve().then(infiniteMicrotasks);
}
```

### **Q4: How does async/await fit in?**

```
async function example() {
  console.log("1");
  await Promise.resolve(); // Pauses here, adds rest to microtask
  console.log("2"); // This is microtask
}
// Equivalent to: Promise.resolve().then(() => console.log("2"))
```

###  **Error Handling Priority**

```
Promise.reject("Error").catch(console.error); // Microtask - runs first
setTimeout(() => console.log("Timeout"), 0); // Macrotask - runs after
// Error logged before Timeout
```

###  **Node.js Specific**



```
// Node.js order: nextTick → Microtasks → Macrotasks
process.nextTick(() => console.log("NextTick")); // Highest priority
Promise.resolve().then(() => console.log("Promise"));
setImmediate(() => console.log("setImmediate")); // Macrotask
// Output: NextTick, Promise, setImmediate
```

###  **UI Updates**


```
// Bad: UI blocks until all microtasks finish
element.addEventListener("click", () => {
  processData(); // Long microtask chain
  updateUI(); // UI won't update until all microtasks done
});

// Better: Yield to browser
element.addEventListener("click", async () => {
  await processData(); // Break with microtasks
  updateUI(); // Can render between microtasks
});

// Best for heavy tasks: Use macrotask
element.addEventListener("click", () => {
  setTimeout(() => { // Let browser render first
    processData();
    updateUI();
  }, 0);
});
```

## **Interview Ready Summary**

"JavaScript has one call stack (single-threaded). Async operations like `setTimeout` and Promises use browser APIs. When ready, their callbacks go to queues:

- **Microtask Queue**: Promises, `queueMicrotask()`. Highest priority.
    
- **Macrotask Queue**: `setTimeout`, events, I/O.
    

The Event Loop constantly checks: if call stack empty → run all microtasks → run one macrotask → repeat.

This explains why `Promise.then()` runs before `setTimeout(fn, 0)`, and why heavy microtasks can block rendering."

**Pro Tip**: In interviews, draw the diagram:

text

[Call Stack] ↔ [Event Loop] ↔ [Microtask Queue]
                          ↕
                    [Macrotask Queue]
                          ↕
                    [Web APIs]

This shows understanding beyond just memorizing order.