
- Function Declaration
	- full function get hoisted
	- can be called before definition
	- function must have name
	- used mostly for public api
```
function add (a,b){
	return a+b
}
```

- Function Expression
	- variable i.e function name get hoisted, not function
	- can not be called before definition
	- this function can be anonymous as well
	- this functions usually used in callback and HOC
```
const name = (a, b) => {
	return a+b
}
```

- In traditional function value of this depend on how function is called - [[this in traditional function]]
- arrow function does not have their own 'this' , it inherit this from surrounding scope
- arrow function can not be constructors and  bad for dynamic this also can not use arguments i.e
```
const sum = () => {
  console.log(arguments);
};

sum(1, 2, 3);
❌ This throws an error:
ReferenceError: arguments is not defined

// will get output {0: 1, 1:2, 2:3, length:3} in traditional function
```

and why arrow function exist

```
function Timer() {
  this.seconds = 0;

  setInterval(() => {
    this.seconds++; // ✅ lexical this
  }, 1000);
}
 // this.seconds will be 1
```

- Note - Arrow function looks for **Function** to get value of this i.e. it should be defined within any outer function 
- Scope Chain & Hoisting
	- when JavaScript looks for variable , it looks if locally first, then in its parents and then its parent's parent and then in global
	- and hoisting means declaration processed before execution, not assignment

- [[this clarity traditional and arrow function doc]]