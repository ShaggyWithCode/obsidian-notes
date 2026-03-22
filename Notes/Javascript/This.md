

## **What is `this`?**

`this` is a special keyword in JavaScript that refers to the **context** in which a function is executed. Its value is determined by **how the function is called**, not where it's written.

Think of `this` like a **"who am I?"** for a function. It tells the function which object it's working with.

---

## **Five Rules for Determining `this`**

There are 5 rules that decide what `this` points to. They have priority order.

### **Rule 1: Global Context (Default)**

In the global scope (outside any function), `this` refers to the global object:

- Browser: `window`
- Node.js: `global`


`javascript`

```
console.log(this); // window (in browser)
var x = 10;
console.log(this.x); // 10 (window.x)
```

In **strict mode**, global `this` in a function is `undefined` (to prevent mistakes).

---

### **Rule 2: Implicit Binding (Object Method)**

When a function is called as a method of an object, `this` refers to that object.

```
const user = {
  name: 'John',
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
};
user.greet(); // "Hello, I'm John" → this = user

**Important:** The object before the dot (`.`) is the context.

javascript

const user2 = {
  name: 'Jane',
  greet: user.greet
};
user2.greet(); // "Hello, I'm Jane" → this = user2
```

---

### **Rule 3: Explicit Binding (call, apply, bind)**

You can force a function's `this` to be whatever you want.

- `func.call(context, arg1, arg2)` – calls function with given `this`
- `func.apply(context, [args])` – same but arguments as array
- `func.bind(context)` – returns **new function** with `this` permanently set

```
function introduce(age) {
  console.log(`I'm ${this.name}, ${age} years old`);
}
const person = { name: 'Alice' };
const person2 = { name: 'Bob' };
introduce.call(person, 25);   // I'm Alice, 25 years old
introduce.apply(person2, [30]); // I'm Bob, 30 years old
const boundIntroduce = introduce.bind(person, 35);
boundIntroduce(); // I'm Alice, 35 years old
```

**Note:** `bind` creates a permanent binding. You cannot change `this` of a bound function again (even with call/apply).

---

### **Rule 4: `new` Binding (Constructor)**

When a function is called with `new`, a new object is created, and `this` refers to that new object.


```
function Person(name) {
  this.name = name;
  this.greet = function() {
    console.log(`Hi, I'm ${this.name}`);
  };
}
const p = new Person('Charlie');
p.greet(); // "Hi, I'm Charlie" → this = p
````

If the constructor returns an object explicitly, that object becomes the result and `this` is discarded. But normally, `this` is the new instance.

---

### **Rule 5: Arrow Functions (Lexical `this`)**

Arrow functions **do not have their own `this`**. They inherit `this` from the surrounding (lexical) scope.

```
const user = {
  name: 'David',
  greet: function() {
    // Regular function
    setTimeout(function() {
      console.log(this.name); // ❌ 'this' is window (or undefined in strict)
    }, 100);
  }
};
user.greet(); // undefined
// Fix with arrow function
const user2 = {
  name: 'David',
  greet: function() {
    setTimeout(() => {
      console.log(this.name); // ✅ 'David' – arrow inherits this from greet's this
    }, 100);
  }
};
user2.greet();
```

**Rule:** Arrow functions capture the `this` value of the enclosing **non-arrow function** at the time they are defined.

---

## **Priority of Rules (from highest to lowest)**

1. **`new` binding** – if called with `new`, `this` is the new object.
2. **Explicit binding** – `call`, `apply`, `bind` override implicit.
3. **Implicit binding** – object method call.
4. **Default binding** – global or `undefined` in strict mode.

Arrow functions ignore all these rules – they use lexical scope.

---

## **Common Pitfalls & Senior Wisdom**

### **1. Losing `this` in callbacks**


```
class Button {
  constructor(text) {
    this.text = text;
  }
  click() {
    console.log(this.text + ' clicked');
  }
}
const btn = new Button('Submit');
setTimeout(btn.click, 1000); // ❌ undefined – called as standalone, this = window
```

**Solutions:**

- Use arrow function in callback: `setTimeout(() => btn.click(), 1000)`
- Bind in constructor: `this.click = this.click.bind(this)`
- Use class field arrow function: `click = () => { ... }`

### **2. `this` in nested functions**


```
const obj = {
  data: [1,2,3],
  process: function() {
    this.data.forEach(function(item) {
      console.log(this); // ❌ window, not obj
    });
  }
};
```

**Fix:** Use arrow function in `forEach`, or store `this` in variable (`const self = this`).

### **3. Arrow functions as methods**


```
const obj = {
  name: 'Eve',
  greet: () => {
    console.log(this.name); // ❌ 'this' is outer scope (window)
  }
};
obj.greet(); // undefined
```

**Insight:** Avoid arrow functions for object methods that need `this` to refer to the object.

### **4. `this` in event listeners**

In DOM event handlers, `this` refers to the element that received the event.

```
button.addEventListener('click', function() {
  console.log(this); // button element
});
```

But if you use arrow function, `this` becomes lexical (probably window), which is usually not what you want.

### **5. `this` in React class components**


```
class MyComponent extends React.Component {
  handleClick() {
    console.log(this); // undefined if not bound
  }
  render() {
    return <button onClick={this.handleClick}>Click</button>;
  }
}
```

**Solution:** Bind in constructor or use class property arrow function: `handleClick = () => {...}`

---

## **Interview Questions & Answers**

### **Q1: What is `this` in JavaScript?**

**Answer:** `this` is a keyword that refers to the context where a function is executed. Its value is determined by how the function is called: global object (default), object before dot (implicit), explicit with call/apply/bind, new instance, or lexical scope for arrow functions.

### **Q2: How do you determine the value of `this`?**

**Answer:** I look at the call site. I check:

- Is the function called with `new`? → new object.
- Is it called with `call`/`apply`/`bind`? → specified context.
- Is it called as a method of an object (obj.func())? → that object.
- Otherwise: global (or undefined in strict mode).
- If it's an arrow function, `this` is inherited from outer non-arrow function.

### **Q3: What's the difference between `call`, `apply`, and `bind`?**

**Answer:** `call` and `apply` invoke the function immediately with a given `this`. `call` takes arguments individually, `apply` takes an array. `bind` returns a new function with `this` permanently set, and it can be called later.

### **Q4: Why does `this` behave differently in arrow functions?**

**Answer:** Arrow functions don't have their own `this`. They capture the `this` value from the enclosing scope at the time of definition. This makes them great for callbacks where you want to keep the outer context.

### **Q5: How would you fix a method losing its `this` when passed as a callback?**

**Answer:** I'd either:

- Wrap it in an arrow function: `() => obj.method()`
    
- Use `bind`: `obj.method.bind(obj)`
    
- In classes, bind in constructor or use class field arrow function.
    

### **Q6: What's the output of this code?**

javascript

const obj = {
  a: 1,
  b: function() {
    console.log(this.a);
  }
};
const func = obj.b;
func();

**Answer:** `undefined` (or error in strict) because `func()` is called without any context, so `this` is global. There's no dot, so implicit binding doesn't apply.

### **Q7: How does `this` work in strict mode?**

**Answer:** In strict mode, the default binding (global) becomes `undefined` instead of the global object. This helps catch accidental global usage.

---

## **Visual Summary**

text

Call Site → How is function invoked?
├── with `new` → new object
├── with `call`/`apply`/`bind` → specified object
├── as method (obj.func()) → object before dot
├── standalone → global (or undefined in strict)
└── arrow function → lexical this from outer scope

**Senior Insight:** Understanding `this` is about understanding **execution context**. Every function call creates a new execution context, and `this` is part of that context. Mastery comes from practicing different call patterns.

---

## **Practical Example (Real-World)**

javascript

class ShoppingCart {
  constructor() {
    this.items = [];
    // Bind method to avoid losing this in event handlers
    this.addItem = this.addItem.bind(this);
  }
  
  addItem(item) {
    this.items.push(item);
    this.render();
  }
  
  render() {
    // render cart UI
  }
}
// In a button click:
button.addEventListener('click', () => {
  cart.addItem('apple'); // works even if addItem is not bound? Actually need to check context.
  // If we pass cart.addItem directly as callback, it would lose this.
  // So either bind in constructor or use arrow in listener.
});

---

## **Interview Ready Summary**

"`this` is a context variable determined by call site. There are 4 main rules: default (global), implicit (object method), explicit (call/apply/bind), and new binding. Arrow functions are special – they inherit `this` lexically. Senior developers always consider how a function is called and use binding techniques to avoid losing context, especially in callbacks and event handlers."