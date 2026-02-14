

- **Memory allocation** = Checking into a hotel room
    
- **Memory usage** = Staying in the room
    
- **Garbage collection** = Hotel staff cleaning rooms after guests leave
    
- **Memory leak** = Guests left but we forgot to clean their rooms
## **Memory Allocation**
JavaScript mainly uses two memory areas
1. Stack - Fast and fixed size
	- Stack Stores Primitives, function calls, references
2. Heap - Slower and Dynamic Size
     - Heap stores Objects, arrays, functions, strings

```
// STACK (Fast, fixed size)
// Stores: Primitives, function calls, references
let age = 30;           // Stack: age = 30
let name = "John";     // Stack: pointer to heap where "John" is stored

// HEAP (Slower, dynamic size)
// Stores: Objects, arrays, functions, strings
let person = {          // Stack: pointer to heap object
  name: "John",        // Heap: object with properties
  age: 30
};

```


### **Memory Allocation in Action**
```
// 1. Primitive values (stack or heap)
let num = 42;          // Stack: allocates space for number
let str = "Hello";     // Stack: pointer, Heap: string "Hello"

// 2. Object allocation (heap)
const user = {         // Stack: pointer to heap address
  id: 1,               // Heap: object with primitives
  name: "Alice",       // Heap: pointer to string "Alice"
  scores: [95, 87, 92] // Heap: pointer to array object
};

// 3. Function allocation
function calculate() {  // Heap: function object
  let total = 0;       // Stack during execution
  return total;
}

// 4. Array allocation
const largeArray = new Array(1000000).fill(0); // Heap: 1 million items
```

