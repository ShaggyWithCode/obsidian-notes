**Iterators**
- Iterator is an special object used to step through collection one item at a time
- iterator object has `next()` method. whenever we call that method it gives a object of properties for value and status flag => `{value:'anyValue' , done: 'true/false' }`
- it makes `iterable` things read friendly and used for memory efficiency.
- we have iterator object on array at key `[Symbol.iterator]`
```

//this is a manual way to explain iterator

const fruits = ['apple', 'banana'];

// Get the iterator object from the array
const fruitIterator = fruits[Symbol.iterator]();

console.log(fruitIterator.next()); // { value: 'apple', done: false }
console.log(fruitIterator.next()); // { value: 'banana', done: false }
console.log(fruitIterator.next()); // { value: undefined, done: true }
```

- `Iterable` is something which has `[Symbol.iterator]` key which gives iterator
```

//custom iterator
//By default a plain object does not have a `next()` method.

const myObject = {
  a: 1,
  b: 2,
  [Symbol.iterator]() {
    const keys = Object.keys(this);
    let index = 0;
    return {
      next: () => {
        if (index < keys.length) {
          const key = keys[index++];
          return { value: [key, this[key]], done: false };
        }
        return { done: true };
      }
    };
  }
};

for (const [key, val] of myObject) {
  console.log(key, val); // Works now!
}
```

**Generators**
- generator is a function which create iterator for you
- generator function create generator object which is both `iterable` and `iterator`
- Generator can pause and resume execution using the yield keyword.
- used to handle infinite data streams and complex async tasks. 
- generators are used for lazy evaluation
i.e 
```
// A Senior-level use case: Unique ID Generator
function* idCreator() {
  let id = 0;
  while (true) {
    yield id++; // Execution stops here until .next() is called
  }
}

const ids = idCreator();
console.log(ids.next().value); // 0
console.log(ids.next().value); // 1
// We could do this forever without a memory leak.
```


```
const user = { name: "Alice", role: "Admin", status: "Active" };

// A generator function that yields entries one by one
function* entryGenerator(obj) {
  for (const entry of Object.entries(obj)) {
    yield entry; // Yields [key, value]
  }
}

const gen = entryGenerator(user);

// Using a while loop to manually call .next()
let result = gen.next();
while (!result.done) {
  const [key, value] = result.value;
  console.log(`Processing ${key}: ${value}`);
  result = gen.next();
}

```

also - You do not strictly need a custom generator function. Since `Object.entries()` returns an array, you can directly grab its built-in iterator.
```
const user = { id: 101, type: "pro" };
const entries = Object.entries(user);
const iterator = entries[Symbol.iterator](); // Get the built-in iterator

// Using a for loop with manual .next() calls
for (let res = iterator.next(); !res.done; res = iterator.next()) {
  console.log(res.value); // [key, value]
}
```

standard way -

```
const myObject = {
  name: "Sam",
  age: 25,
  role: "Developer",
  
  // Adding the generator iterator
  *[Symbol.iterator]() {
    const entries = Object.entries(this);
    for (const entry of entries) {
      yield entry; // Yields [key, value] pairs
    }
  }
};

// Now you can use it directly in a for...of loop
for (const [key, value] of myObject) {
  console.log(`${key}: ${value}`);
}

// Or use the spread operator
const entriesArray = [...myObject]; // [["name", "Sam"], ["age", 25], ["role", "Developer"]]
```


to assign generator to `myObject` we can do -



```

const myObject = { a: 1, b: 2 };

// Correct way to assign a generator function externally
myObject[Symbol.iterator] = function* () {
  for (const key of Object.keys(this)) {
    yield [key, this[key]];
  }
};

// Usage
for (const [key, value] of myObject) {
  console.log(key, value);
}
```


Caveats - 

### A. Non-Rewindable

Generators are "one-way streets." Once you consume a value, it’s gone. If you need to see the first item again, you have to create a brand-new generator instance. You cannot "seek" or "index" a generator like an array.

### B. The `yield*` Delegate

Seniors use `yield*` to delegate to another generator. This allows you to compose complex logic from smaller, reusable generators.

JavaScript

```
function* subTask() {
  yield "Step 2";
}

function* mainTask() {
  yield "Step 1";
  yield* subTask(); // Delegates control
  yield "Step 3";
}
```

### C. Async Generators

In modern Node.js or high-performance frontend apps, we use `async function*`. This allows you to iterate over data that arrives over the network (like a stream of chunks from an API) using a simple `for await...of` loop.