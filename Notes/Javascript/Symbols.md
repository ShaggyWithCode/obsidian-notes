- Symbols provide a way to add unique and invisible property to the object that can never clash with another property names, even if those names are identical . 
```

const sym1 = Symbol();
const sym2 = Symbol('discription');
const sym3 = Symbol('discription');

console.log(sym1 === sym2) //false
console.log(sym3 === sym3) //false
```

### **Way to use symbol**

1.**Avoid Property Name Collision**
- when multiple libraries/module works on same object
```
//library 1

const cache_key = Symbol('cache')
object[cache_key] = 'library one data'

//library 2
const cache_key = Symbol('cache')
object[cache_key] = 'library two data'

==> both can co-exist without overriding each other data
```
- Hidden Properties
```

//symbol doesn't appear in normal iteration 

const user = {
  name: 'John',
  age: 30,
  [Symbol('internalId')]: 12345 // Evaluates Symbol('internalId') first, then uses result as key also can't access it by . (its for string key only)
};

console.log(Object.keys(user)); // ['name', 'age']
console.log(Object.getOwnPropertySymbols(user)); // [Symbol(internalId)]

for (let key in user) {
  console.log(key); // 'name', 'age' only
}



DOT NOTATION (.key) → For string property names
BRACKET NOTATION ([expression]) → For dynamic/computed property names
                       ↓
                  Variables, Expressions, or SYMBOLS
                  
```
- Well Known Symbols (built in)
```
// Symbol.iterator - Makes object iterable
const myIterable = {
  [Symbol.iterator]: function* () {
    yield 1;
    yield 2;
    yield 3;
  }
};

console.log([...myIterable]); // [1, 2, 3]

// Symbol.toStringTag - Customize Object.toString()
class MyClass {
  get [Symbol.toStringTag]() {
    return 'MyCustomClass';
  }
}
console.log(new MyClass().toString()); // [object MyCustomClass]
``` 
- Use in Constants (Enums Pattern)
```
// Before Symbols - strings can collide
const DIRECTIONS = {
  UP: 'UP',
  DOWN: 'DOWN'
};
// Someone else could use 'UP' and cause bugs

// With Symbols - truly unique
const DIRECTIONS = {
  UP: Symbol('UP'),
  DOWN: Symbol('DOWN')
};
// No possible collision
```
- Important Symbol Methods
```
const sym = Symbol('mySymbol');

// Get description
console.log(sym.description); // 'mySymbol'

// Global Symbol Registry - create key globally -exeption the fact that symbols are unique

const sym1 = Symbol.for('globalKey'); // Creates or retrieves
const sym2 = Symbol.for('globalKey');
console.log(sym1 === sym2); // true - Same symbol from registry

Symbol.keyFor(sym1); // 'globalKey' - Get key from global symbol
```




