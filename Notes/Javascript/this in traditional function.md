- `this` would be window object for browser and global object in `nodejs`  and `undefined` in strict mode for given example  why? - the function called directly , there is no owner object for it
```
function show(){
console.log(this)
}

show()
```

in this example it NaN as as well as in there is no increment prop on window object so it will become undefined and arithamatic on undefind will be NaN
```
class Timer { 
		constructor() {
		 this.seconds = 0;
		  } 
		  // Traditional function creates its OWN 'this', which refers to the global/window object 
		  
		  startBad() { 
			setInterval(function() { 
			// 'this' here is NOT the Timer instance! 
			console.log(this.seconds++); 
			// Output: NaN, because this.seconds is undefined }, 1000); 
			} 
			
			// Arrow function uses 'this' from the lexical (surrounding) scope (the Timer instance) 
			
			startGood() { 
				setInterval(() => { 
				// 'this' here IS the Timer instance! 
				console.log(this.seconds++); 
				// Output: 0, 1, 2...
				 }, 1000); 
	} 
}
```

object method call gives this as a object 

```
const user = {
	name: "pikachu",
	show(){
		console.log(this)
	}
}

user.show()
this == user here 
```

and 

```
function show(){
	console.log(this)
}

const user1 = {name: 'pikachu', show}
const user1 = {name: 'raichu', show}

//if we call function in both of object that value of this would be diffrernt 
HENCE IT DOES NOT BELONGS IN FUNCTION, IT BELONGS TO CALL SITE
```

case of detached function

```

const user = {
  name: "Sagar",
  show() {
    console.log(this.name);
  }
};

const fn = user.show;
fn(); // ❌ undefined

why ?
- You removed the object
    
- Call becomes `fn()`
    
- No owner → `this` lost
```

```

const user = {
  name: "Sagar",
  show() {
    setTimeout(function () {
      console.log(this.name);
    }, 1000);
  }
};

user.show(); // ❌ undefined
Why?

""setTimeout"" calls the function

Not user

So this ≠ user
```

ESSENCE -

```
Don’t ask “what is `this` inside the function?”  
Ask “HOW was this function CALLED?”
```

