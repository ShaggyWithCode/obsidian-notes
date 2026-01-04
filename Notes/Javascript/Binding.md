
- we have `call`, `apply` and `bind` for `this` binding - these methods allow us to manually set value of `this` for function borrowing
	- when we call method without object i.e. `obj.method()` , that time we need to care about `this` as this become undefined there and issue happens
```
function greet() {
  return `Hi ${this.name}`;
}

const user = { name: "Sagar" };

greet(); // ❌ undefined

- `greet()` is called **by itself**
    
- No object on the left side
    
- So `this` is:
    
    - `undefined` (strict mode)
        
    - `window` (non-strict)
----------------------------------------------------------------------------
const user = {
  name: "Sagar",
  greet() {
    return `Hi ${this.name}`;
  }
};
// or we can add greet on prototype as well
user.greet(); // ✅ "Hi Sagar"
```
- in given example(1st) above you can see that function called without object 
- now lets look into issues when we have class
```
class User {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hi ${this.name}`;
  }
}

const user = new User("Sagar");
This works:

js
Copy code
user.greet(); // ✅ "Hi Sagar"
----------------------------------------------------------------------------

const greetFn = user.greet;
greetFn(); // ❌ TypeError / undefined //same like function
Why this breaks
- greetFn() is a plain function call
- The object (user) is gone
So this is lost
👉 Classes do NOT auto-bind methods

## Even worse: callbacks (real-world bug)

setTimeout(user.greet, 1000); // ❌ loses `this` 

Why?

- `setTimeout` calls the function
    
- Not `user`
    
- So `this` is not `user`
```
- here point is if our function is assigned to another variable and even called(traditional function) inside other function, the this gets lost. solution => use arrow function there ;) 
- So, Question is when do we really need binding 
	- When passing methods as callback i.e
		`const { greet } = user;
		`greet(); // ❌`

	- when `destructuring` methods
		`const { greet } = user;`
		`greet(); // ❌`
		
	- when storing functions into variables 
		`const fn = user.greet;`
		`fn(); // ❌`

- How to fix 
	- by binding it - while callback and for methods in class(3rd point as well for class)
		`const bindgreet = user.greet.bind(user)`
	- by arrow function wrapper - at call site
		`setTimeout(()=>"pikachu", 1000)`
	- by adding arrow function in class
	`	class User {
		  `name = "Sagar";

		  greet = () => {
		    return `Hi ${this.name}`;
		  };
		}

- **call** : it invoke the function immediately  and argument passed comma separated
```
greet.call(user, arg1, arg2);
```
- apply:  it invoke the function immediately, and argument passed as array.
```
greet.apply(user, [arg1, arg2]);
```
- bind: return new function. this permanently fixed.
```
const boundGreet = greet.bind(user);
boundGreet();
```

![[unnamed.png]]


![[NotebookLM Mind Map (3).png]]