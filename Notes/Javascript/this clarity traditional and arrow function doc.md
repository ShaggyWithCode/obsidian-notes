
- “if we have an arrow function inside object, then this inside it would be undefined as it's at top level and it's not in function, it is in object. at top level its undefined there”
```
const obj = {
  x: 10,
  fn: () => console.log(this.x) // ❌ undefined
};

```

- “but if that arrow function inside function and that function inside object , then as that function will run when its called on object , hence it inherit its this and give it to arrow function.”
```
const obj = {
  x: 10,
  fn() {                 // normal function
    const arrow = () => {
      console.log(this.x); // arrow copies `this`
    };
    arrow();
  }
};

obj.fn(); // 10

```

- **for arrow function one should keep in mind that it should be in function to get definite value of this**
- with traditional function we should look for is there anything on left it is called on, if not then this will undefined.
```
obj.fn();   // this = obj
fn();       // this = undefined (in strict / class)
```


### 🔹 Core Truths (Non-Negotiable)

1. **Objects do NOT create `this`**
    
2. **Functions create `this`**
    
3. **Arrow functions do NOT create `this`**
    
4. **Arrow functions copy `this` from nearest normal function**
    

---

### 🔹 Arrow Function Rules (Golden Rules)

```
// ❌ Arrow at top-level (no function) 
const obj = {   
fn: () => console.log(this) // undefined 
};
````

✔ Because:

- No surrounding function
    
- Object does NOT count
    

---

```
// ✅ Arrow inside normal function 
const obj = {   
	fn() {     
		const arrow = () => console.log(this);     
	arrow();  
	} 
};


obj.fn(); // this = obj
````

✔ Because:

- Arrow copies `this` from `fn()`
    

---

### 🔹 Traditional Function Rules (Golden Rule)

> **Look at how the function is CALLED**

```
obj.fn();   // this = obj 
fn();       // this = undefined`
```

If **nothing is on the left**, `this` is lost.

---

### 🔹 Class Case (Same Rules Apply)

```
class Timer {   
	startBad() {
	     setInterval(function () {
	            console.log(this.seconds); // ❌ lost
	    });   
	}    
	startGood() {
	     setInterval(() => {
	            console.log(this.seconds); // ✅ copied     
	    });   
	}
}
````

✔ Classes do **NOT** change `this` rules  
✔ Arrow protects `this`

![[NotebookLM Mind Map (2).png]]