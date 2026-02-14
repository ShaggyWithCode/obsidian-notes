
#### **Q1: What happens when we call a function?**
Ans - JavaScript creates new execution context with two phases. First - Creation Phase: sets up variable environment, lexical environment and this. Second: Execution phase: runs code line bu line, this context is pushed into call stack.

#### **Q2: What's the difference between execution context and scope?**
Ans - scope is about visibility - what variable you can access. Execution context is 
about execution - its a environment where code runs.

#### **Q3: How does JavaScript find variables?**
Ans -  Through scope chain. look into current execution context's variable environment , if not then look into the outer refence of parent context, and so on until global . If not anywhere then its - `ReferenceError`

#### **Q4: Why do we get "Cannot access before initialization" with let/const?**
Ans-  Because of TDZ (Temporal dead zone). during creation phase let/const allocation happens but not initialized . 



### *Execution Context

Every time function runs, JavaScript create an execution context - the "universe/environment" where that function operates.

### **3 types of context**
**1. Global Execution context (GEC)**
- created when script starts running
- Represent Global scope (`window` in browser, `global` in nodejs)
**2. Function execution context (FEC**)
- created each time function is called
```
function sayHello() {
  // New FEC created here
  console.log("Hello");
}
```

### **eval execution context**
- rare one , avoid


## *What's inside execution Context ?
- it create `context object` for each execution
```
function calculateTotal(price, quantity) {
  const taxRate = 0.08;
  const subtotal = price * quantity;
  const total = subtotal * (1 + taxRate);
  return total;
}

// When calculateTotal(10, 2) is called:
ExecutionContext = {
  // PHASE 1: Creation Phase (Setup)
  // PHASE 2: Execution Phase (Run code)
}
```

### two phases of context
**1. creation phase**
in creation phase javascript sets up -
1. Variable Environment  (var, function declaration - hosting happens here)
2. Lexical Environment (let, const, functions in es6)
3. `this` binding 

```
function example(a, b) {
  var x = 10;
  let y = 20;
  const z = 30;
  
  function inner() {
    console.log("Inner");
  }
}

// During Creation Phase for example():
ExecutionContext = {
  // 1. VariableEnvironment (old style - var, function declarations)
  //    HOISTING happens here
  VariableEnvironment: {
    a: undefined,     // arguments initialized
    b: undefined,
    x: undefined,     // var declarations hoisted
    inner: <function> // function declarations fully hoisted
  },
  
  // 2. LexicalEnvironment (let, const, functions in ES6+)
  //    Also includes reference to outer scope
  LexicalEnvironment: {
    y: <uninitialized>, // TDZ starts (Temporal Dead Zone)
    z: <uninitialized>,
    outer: <reference to parent scope>,
    this: <determined>
  },
  
  // 3. this Binding
  this: window // (or undefined in strict mode, or object if method)
}
```

**points to consider**
- variable a and b are argument which comes under variable Environment context of function
- in Lexical Environment we get a reference to parent scope i.e outer is internal reference to parent scope
- in variable environment , variable assigned `undefined` and function hoist with its declaration
- in lexical environment, variables are uninitialized 

**2. execution phase**
here code runs

```
function example(a, b) {
  console.log(a, b); // "first", "second" - arguments assigned
  
  var x = 10;        // x now assigned 10
  let y = 20;        // y initialized and assigned 20 (TDZ ends)
  const z = 30;      // z initialized and assigned 30 (TDZ ends)
  
  console.log(x + y + z); // 60
}

example("first", "second");

// During Execution Phase:
ExecutionContext = {
  VariableEnvironment: {
    a: "first",     // Arguments now have actual values
    b: "second",
    x: 10,          // var assigned
    inner: <function>
  },
  
  LexicalEnvironment: {
    y: 20,          // let assigned (TDZ ends)
    z: 30,          // const assigned (TDZ ends)
    outer: <...>,
    this: window
  }
}
```

### **CALL STACK**
- its a data structure (Last In First Out) that track execution context
```
function first() {
  console.log("First starts");
  second();
  console.log("First ends");
}

function second() {
  console.log("Second starts");
  third();
  console.log("Second ends");
}

function third() {
  console.log("Third starts");
  console.log("Third ends");
}

first();

// Call Stack Visualization:
// Step 1: first() called
// Stack: [Global Context, first() Context]
//
// Step 2: second() called from first()
// Stack: [Global Context, first() Context, second() Context]
//
// Step 3: third() called from second()
// Stack: [Global Context, first() Context, second() Context, third() Context]
//
// Step 4: third() finishes
// Stack: [Global Context, first() Context, second() Context]
//
// Step 5: second() finishes
// Stack: [Global Context, first() Context]
//
// Step 6: first() finishes
// Stack: [Global Context]
```


### *CLOSURES & EXECUTION CONTEXT
- closures happens because reference to parent environment(lexical environment) is always created from inner function. 
- execution context remember its scope chain means inner function remember outer scope - ALWAYS - even outer function finish
```
function createCounter() {
  // Execution context for createCounter created
  let count = 0; // In VariableEnvironment of createCounter's context
  
  return function() {
    // This function's scope chain remembers createCounter's context
    count++; // Can still access count even though createCounter finished
    return count;
  };
  // Normally, createCounter's context would be destroyed
  // But the returned function maintains a REFERENCE to it
}

const counter = createCounter();
// createCounter execution finishes BUT its context is NOT garbage collected
// because the returned function still references it

console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3

// Each call creates new execution context for the inner function
// All of them share the SAME parent context (where count lives)
```

### **"THIS" BINDING IN EXECUTION CONTEXT**

The value of `this` is determined when execution context is created:
```
// Rule 1: Default binding (global or undefined)
function standalone() {
  console.log(this); // window (non-strict) / undefined (strict)
}

// Rule 2: Implicit binding (object method)
const obj = {
  name: "John",
  sayName() {
    console.log(this.name); // "John" - this = obj
  }
};

// Rule 3: Explicit binding (call/apply/bind)
function showId() {
  console.log(this.id);
}
const obj2 = { id: 42 };
showId.call(obj2); // 42 - this = obj2

// Rule 4: "new" binding (constructor)
function Person(name) {
  this.name = name; // this = new object being created
}
const p = new Person("Jane");

// Rule 5: Arrow functions (lexical this - from parent scope)
const arrowObj = {
  name: "Arrow",
  traditional: function() {
    console.log(this.name); // "Arrow" - this = arrowObj
  },
  arrow: () => {
    console.log(this.name); // "" or undefined - this = global/window
  }
};
```


remember this -  Arrow functions (lexical this - from parent scope)

#### **Nested Functions and Memory**
```
// PROBLEM: Each outer() call creates NEW inner() function
function outer() {
  const bigArray = new Array(1000).fill("data");
  
  return function inner() {
    console.log(bigArray.length);
    // inner maintains reference to outer's context
    // bigArray stays in memory!
  };
}

const funcs = [];
for (let i = 0; i < 1000; i++) {
  funcs.push(outer()); // Creates 1000 bigArrays in memory!
}

// SOLUTION: Avoid unnecessary closures
function createLightweight() {
  // No closure formed
  return function() {
    console.log("Light");
  };
}
```


```
Code Execution Flow:
1. Parse script → Global Execution Context created
2. Execute global code → Functions called
3. For each function call:
   a. Create new execution context
   b. Push to call stack
   c. Run function code
   d. Pop from stack when done
4. Repeat until stack empty

Variable Lookup:
Current Context → Parent Context → Global Context → Error
```

```
┌─────────────────────────────────────────────────┐
│            CALL STACK (LIFO)                    │
├─────────────────────────────────────────────────┤
│  Execution Context 3 (inner function)           │
│  - VariableEnvironment                          │
│  - LexicalEnvironment → outer → global          │
│  - this binding                                 │
├─────────────────────────────────────────────────┤
│  Execution Context 2 (middle function)          │
│  - VariableEnvironment                          │
│  - LexicalEnvironment → global                  │
│  - this binding                                 │
├─────────────────────────────────────────────────┤
│  Execution Context 1 (global)                   │
│  - VariableEnvironment (global vars)            │
│  - LexicalEnvironment (null)                    │
│  - this = window/global                         │
└─────────────────────────────────────────────────┘
```

