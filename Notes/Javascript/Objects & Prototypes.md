- Object literals, constructors, classes
	- JavaScript has Objects, not classes. classes are just a syntax on top of prototypes.
	- object creation style
		- Object literal (Most common)
			`const user = {`
			  `name: "Sagar",`
			  `greet() {`
			    `return Hi ${this.name};`
				`}`
			`};`
	
		- Constructor Function (pre-ES6)
			`function User (name) {`
					`this.name = name`
				`}`
			`User.prototype.greet = function(){
				`return `Hi ${this.name}````
			}
			const u1 = new User("Sagar")
			- here `new` create new object, bind `__proto__`  of that object to `User.prototype`, then bind `this` as well, and then return `object`
			
		- ES6 Class (Modern syntax)
			`class User {`
			  `constructor(name) {`
				`this.name = name;`
			  `}`
			  `greet() {`
				`return Hi ${this.name};`
			  `}`
			  `static type() {`
			    `return "USER";`
			  `}`
			`}`
			- So basically at the end its compile to prototype only

- Prototypes
	- every object has hidden link to another object - thats prototype
	 ex. we get push method on array we created i.e arr= []; arr.push()
	- for that it looks into prototypes , how => arr.__proto_ => Array.prototype 
	- so basically there is internal linking to other object prototype which has all common functions stored to use until that chain become null.
	
		