#### Problem
regular Map/Set holds strong reference to **`objects`**, preventing garbage collection. i.e when we use object as key in Map/Set and when we remove that object, its reference stays there which don't let it for garbage collection. 
```
const obj = { name : "hello"}

const map = new Map();
map.set(obj, "hakuna matata")

obj = null // we don't need object anymore

//But Map still has reference,so { name : "hello"} can't be garbage collected
```

#### Solution - `WeakMap`

`WeakMap` key must be `Object`

```
const weakMap = new WeakMap();
let obj = { id: 1 };

// Set and get like Map
weakMap.set(obj, 'private data');
console.log(weakMap.get(obj)); // 'private data'

// Remove reference
obj = null;
// Now { id: 1 } CAN be garbage collected
// weakMap entry automatically removed
```

#### `WeakMap` Limitations
```
const weakMap = new WeakMap();

// NO iteration methods
// weakMap.keys() ❌ Error
// weakMap.values() ❌ Error
// weakMap.entries() ❌ Error
// weakMap.size ❌ Doesn't exist
// weakMap.forEach() ❌ Doesn't exist

// Why? Because garbage collection could happen anytime
//

```

##### **WeakMap Use Cases**
**1. Private Object Metadata**
```
const privateData = new WeakMap()
class User {

	constructor(name){
		privateDate.set(this, {
		username: name, password: generatePassword()})
	}
	
	getUserName(){
		return privateData.get(this).userName
	}
}

const user = new User('John');
console.log(user.username); // undefined
console.log(user.getName()); // 'John'

// When user instance is garbage collected i.e user = null, its private data is too
```

**2. DOM Element Metadata (Browser)**
```

const elementMetadata = new WeakMap();
const button = document.getElementById('myButton');

elementMetadata.set(button, {
  clickCount: 0,
  lastClicked: null
});

button.addEventListener('click', function() {
  const meta = elementMetadata.get(this);
  meta.clickCount++;
  meta.lastClicked = Date.now();
});

// When button removed from DOM → garbage collected → metadata auto-cleaned
```

**Caching with Automatic Cleanup**
```
const cache = new WeakMap();

function processObject(obj) {
  if (cache.has(obj)) {
    return cache.get(obj);
  }
  
  const result = expensiveComputation(obj);
  cache.set(obj, result);
  return result;
}

// When obj no longer needed → cache entry automatically removed
```



### `WeakSet`

`WeakSet` only stores object, its weakly referenced

```

const weakset = new WeakSet();

weakset.add(obj1)
weakset.add(obj2)
weakset.add(obj3)

console.log(weakSet.has(obj1)); // true

obj1 = null;
// obj1 can be garbage collected, automatically removed from weakSet

obj2 = null
// obj2 can be garbage collected, automatically removed from weakSet

and so on ... point is... it doesn't store the reference at all

to understand this concept, we must know that Map/Set store reference even if assign null to them 
reason - Map/Set used as database to store data, considered as source of data
- also Map/Set used when iteration is needed and wantered to store string and number as keys.

```

 **Tracking Without Preventing GC**

```
javascript

const processedObjects = new WeakSet();

function markAsProcessed(obj) {
  processedObjects.add(obj);
}

function isProcessed(obj) {
  return processedObjects.has(obj);
}

// Objects can still be garbage collected when no longer needed
```

### Interview Questions - 

 **When would you use Symbol over string for object keys?**
 - when need guaranteed `uniqueness` to avoid `key` collisions specially in different libraries 
 - where we don't wanted to show metadata when we `Json.stringify()' or for..in loop or in Object.keys()
 - for example multiple teams work on same codebase, and we use symbol to keep data private (ex. for flags) which will not interfere with public API (`public API`-output that public see)
 ```
 //inside developers libarary code 
 const validated = Symbol('is_validated')
 
 const function initializeUser(user){
	 user[validated] = true
 }
 
  const function isValidated(user){
	 user[validated] == true
 }
 
 //inside codebase 
 
 import { initializeUser } from './library.js';
 let myUser = { name: 'Alice', role: 'admin' }; 
 initializeUser(myUser); 
 
 // 1. COLLISION PROTECTION 
 myUser.VALIDATED = false; // This is a string key; it DOES NOT affect the Symbol! 
 
 // 2. JSON CLEANLINESS 
 console.log(JSON.stringify(myUser)); // Output: {"name":"Alice","role":"admin"} 
 
 // The Symbol is hidden! No internal trash sent to the server. 
 
 // 3. LOOP CLEANLINESS for (let key in myUser) { console.log(key); // Only prints "name" and "role" }
 
 
 
 /// as we see here, the code we added made no change and detect nothing in myUser  about isValidated that we added 
 ```


**Q2: What's the real benefit of `WeakMap` over closure for private data?**
=> With closure each instance create its own function scope(bubble), consuming more memory.
the bubble theory, closure remember variable form outsider function, so, JavaScript engine can not delete outer variable, so it has to wrap that outer variable with bubble (scope) and attach it to inner function. normally for normal function JavaScript delete its local variable when function finished, this can't happens with closure. 
with `WeakMap` all instance share same map, but each entry clear when its garbage collected , so its memory efficient !.
the difference is a memory parking which is more for closure 

```
// Closure approach (more memory per instance)
class User {
  constructor(name) {
    let privateName = name;
    this.getName = () => privateName;
  }
}

// WeakMap approach (shared storage)
const privateData = new WeakMap();
class User {
  constructor(name) {
    privateData.set(this, { name });
  }
  getName() {
    return privateData.get(this).name;
  }
}
```

**Q3: Can you enumerate WeakMap keys if you really need to?**
Ans - NO

**Q4: How do Symbols help with security?**
-Symbol create hidden props, non-enumerable and avoid accidental access as well 


**create completely unique constants**
```

const abc = {
	a : Symbol('a'),
	b : Symbol('b')
}

const enabledFeatures = new Set();
enabledFeatures.add(abc.a);

// No string comparison bugs, completely unique
```

Memoisation with Auto-Cleanup

```
const memoize = (fn) => {
  const cache = new WeakMap();
  
  return (obj) => {
    if (!cache.has(obj)) {
      cache.set(obj, fn(obj));
    }
    return cache.get(obj);
  };
};

const expensiveTransform = memoize((obj) => {
  // ... heavy computation
  return transformed;
});

expensiveTransform({a:1}) //calling function -> it will look for cache -> function with heavy calculation will execute accordingly
```