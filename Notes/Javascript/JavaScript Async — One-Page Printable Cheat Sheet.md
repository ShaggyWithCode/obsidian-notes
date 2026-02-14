
---
**Goal:** Understand callbacks, Promises, async/await, event loop, React async illusion, stale state — without confusion.

## 1️⃣ Core Truth (never forget)

> **JavaScript never waits.  
> It only executes now or schedules work for later.**

---

## 2️⃣ Execution Model

- **Single call stack**
    
- **One JS function runs at a time**
    
- JS is **non-blocking for I/O**, but **blocking for CPU work**
    

---

## 3️⃣ Call Stack (NOW)

- Executes synchronous JS
    
- Grows only when:
    
    `a() { b(); }`
    
- Does NOT grow for:
    
    `a(); b();`
    

---

## 4️⃣ Async Exists Because JS Cannot Block

JS cannot pause for:

- network
    
- timers
    
- disk
    

So the environment says:

> “Give me a function, I’ll call it later.”

That function = **callback**

---

## 5️⃣ Callbacks (raw primitive)

`getData(cb);`

- Push data forward (no return)
    
- Inverts control flow
    
- Leads to callback hell
    

---

## 6️⃣ Promises (structured callbacks)

`getData().then(value => ...)`

Facts:

- `.then()` registers a callback
    
- `.then()` **creates a new Promise**
    
- `return x` inside `.then()` → resolves next Promise with `x`
    

> Promises do NOT remove callbacks — they standardize them

---

## 7️⃣ async / await (syntax sugar)

`const x = await f();`

Means:

`f().then(x => { /* continue */ });`

### Key rules

- `await` **suspends only the current function**
    
- JS keeps running
    
- Continuation runs as a **microtask**
    

> **async functions ALWAYS return Promises**

---

## 8️⃣ You NEVER get async value directly

❌ Impossible:

`const x = f(); // Promise`

✅ Correct:

`await f(); f().then(...)`

---

## 9️⃣ Event Loop (environment responsibility)

Loop:

`1. Run sync JS 2. Drain ALL microtasks 3. Browser may paint 4. Run ONE macrotask 5. Repeat`

### Queues

|Queue|Contains|
|---|---|
|Microtask|Promise.then, await continuation|
|Macrotask|setTimeout, events, IO|

⚠️ DOM updates are **sync**, NOT tasks

---

## 🔟 “Non-blocking” clarified

|Case|Blocks JS?|
|---|---|
|fetch / await|❌|
|Promise.then|❌|
|long loop|✅|
|heavy sync work|✅|

JS doesn’t wait — it gets **busy**

---

## 1️⃣1️⃣ Closures & “capture”

- Closures capture **variable bindings**
    
- Event loop queues **functions**, not values
    
- Functions carry references → snapshots possible
    

---

## 1️⃣2️⃣ Stale State (precise definition)

> **Using an old snapshot after a delay**

Requires ALL:

1. Snapshot (closure / render)
    
2. Delay (event loop / batching)
    
3. Shared state
    

### Not the same as race condition

|Stale State|Race Condition|
|---|---|
|Old snapshot|Competing async tasks|
|Deterministic|Timing-dependent|

---

## 1️⃣3️⃣ React Async Illusion

React rules:

- Render is **always synchronous**
    
- Async happens **outside render**
    
- State update → re-render later
    

Result:

> Render always sees **real data**, never Promises

React does **not await** — it **re-executes render**

---

## 1️⃣4️⃣ Why React batches state

`setCount(count + 1); setCount(count + 1); // stale`

Fix:

`setCount(prev => prev + 1);`

Because:

- state is snapshot per render
    
- updates applied after handler finishes
    

---

## 🔑 Final Mental Model (one line)

> **JavaScript schedules continuations; React re-runs synchronous code when async data arrives.**

---

---

# 🧩 Diagram-Only Version (Visual Memory)

### 1️⃣ Overall Architecture

![https://media2.dev.to/dynamic/image/width%3D800%2Cheight%3D%2Cfit%3Dscale-down%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7dlps29vwzbuskebux0b.png](https://media2.dev.to/dynamic/image/width%3D800%2Cheight%3D%2Cfit%3Dscale-down%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F7dlps29vwzbuskebux0b.png)

![https://media.geeksforgeeks.org/wp-content/uploads/20250208123836185275/Event-Loop-in-JavaScript.jpg](https://media.geeksforgeeks.org/wp-content/uploads/20250208123836185275/Event-Loop-in-JavaScript.jpg)

---

### 2️⃣ Call Stack + Queues + Event Loop

![https://media2.dev.to/dynamic/image/width%3D1600%2Cheight%3D900%2Cfit%3Dcover%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F1v05yqyxbjfiepzphyph.png](https://media2.dev.to/dynamic/image/width%3D1600%2Cheight%3D900%2Cfit%3Dcover%2Cgravity%3Dauto%2Cformat%3Dauto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2F1v05yqyxbjfiepzphyph.png)

![https://media.licdn.com/dms/image/v2/D5612AQHIuZDc3cqPtg/article-cover_image-shrink_600_2000/article-cover_image-shrink_600_2000/0/1721189705579?e=2147483647&t=7z1ivEBMlIOpeq4P2UUbbrj1T64ysIpkPv27efVvq60&v=beta](https://media.licdn.com/dms/image/v2/D5612AQHIuZDc3cqPtg/article-cover_image-shrink_600_2000/article-cover_image-shrink_600_2000/0/1721189705579?e=2147483647&t=7z1ivEBMlIOpeq4P2UUbbrj1T64ysIpkPv27efVvq60&v=beta)

---

### 3️⃣ Promise & async/await Desugaring

![https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AHZbUgpQC_ecQ1UIN_2xdfg.png](https://miro.medium.com/v2/resize%3Afit%3A1400/1%2AHZbUgpQC_ecQ1UIN_2xdfg.png)

![https://nikgrozev.com/images/blog/async-await/SimplePromiseExample.png](https://nikgrozev.com/images/blog/async-await/SimplePromiseExample.png)

---

### 4️⃣ Microtask vs Macrotask Order

![https://media.licdn.com/dms/image/v2/D5612AQG4-RLmRI3OjA/article-cover_image-shrink_600_2000/article-cover_image-shrink_600_2000/0/1737475242051?e=2147483647&t=NkPapdowJMOFOLc_5imHm21QPFA8J3XQWHZ-ke6LD8s&v=beta](https://media.licdn.com/dms/image/v2/D5612AQG4-RLmRI3OjA/article-cover_image-shrink_600_2000/article-cover_image-shrink_600_2000/0/1737475242051?e=2147483647&t=NkPapdowJMOFOLc_5imHm21QPFA8J3XQWHZ-ke6LD8s&v=beta)

![https://files.codingninjas.in/article_images/microtask-and-macrotask-in-javascript-1-1653228810.webp](https://files.codingninjas.in/article_images/microtask-and-macrotask-in-javascript-1-1653228810.webp)

---

### 5️⃣ React Async Rendering Timeline

![https://i.sstatic.net/hvnin.jpg](https://images.openai.com/thumbnails/url/ND3FYHicu5mZUVJSUGylr5-al1xUWVCSmqJbkpRnoJdeXJJYkpmsl5yfq5-Zm5ieWmxfaAuUsXL0S7F0Tw6scvEL9nGJjEhPjnStyCysCPDS1XVPNHYNzYuoKE71dzLPC_KqsCjM88v2cXcKr4pUKwYAXfwmQQ)

![https://blog.logrocket.com/wp-content/uploads/2023/02/react-hooks-lifecycle.png](https://blog.logrocket.com/wp-content/uploads/2023/02/react-hooks-lifecycle.png)