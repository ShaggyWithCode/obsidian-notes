
 all this happens in creation phase where memory js scan the code, allocate memory, decide scope and Hoists declaration
 
1.  **var** (the legacy keyword)
	- Original way of declare of variable in JavaScript
	- it has **behavior** which often lead to unexpected issues in **large** codebase which is why let and cost are introduced.
	- var scope is function/global scoped
	- var is function scoped means it belongs in function i.e var variable declared/ initiated inside function will not hoist or available outside the function.
	- its **declaration** is hoisted, **assignment** is not
	- it doesn't allow block structure rules 
	- declaration variable hoists **to the top of their scope** with undefined value.
	- redeclaration allowed , reassignment allowed.
	```
	console.log(a) //print undefined 
	var a = 4
	
	//what js does
	
	var a
	console.log(a)
	a = 4
	```

	- variable with no keyword considered as global variable if it is In non-strict mode.
	- in given example below , var become global copy which upgrades with loop and when settimeout start printing console the value of that time would be 3
```
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 1000);
}
// 3, 3, 3
```



2. Let
		- Mutable
		- block scope, hence safe
		- `ReferenceError` if try to access outside of scope or before declaration 
		- `SyntaxError` on redeclaration 
		- Reassignment allowed
		- actually Let also hoisted in its scope, reason - to catch error as its gives error there . is stay in TDZ till installation 
		- loop issue with var gets solved with let as it has value of i iteration wise 
		- issue with var happens when variable shadowing happens 
```
let count = 10;

if (true) {
  let count = 20;
  console.log(count); // 20
}

console.log(count); // 10
```



3. Const 
	      - same scoping rules as let
	      - TDZ applied here too
	      - no reinitialization and reassignment not allowed 
	      - value of object and array can be update in const i.e mutation allowed for array and object BUT can not assign new array/object to const variable 


- so full hoisting happens with var
- for let/const hoisting happens in TDZ(temporal dead zone), but not initialized. if we try to access from TDZ, it gives `ReferenceError`.
-