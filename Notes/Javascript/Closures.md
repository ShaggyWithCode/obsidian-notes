- closure means a function remembering variable from its creation scope i.e from outer function
- here outer function returns a function which remembers variables from outer function. 
```
function outer(){
	let secret = 42
	
	return function inner(){
		return secret
	}
}

const fn = outer() //outer function done running here

fn() // return 42
```

- we achieve `Encapsulation/private state` using closure like this - 
```
function createCounter () {
	let count = 0
	
	return {
		get(){
			return count
		}
		
		inc(){
			return count++
		}
	}
}

const counter = createCounter()

counter.inc()  
counter.get()  //1


// here count has become private, we can not access directly, this is data hiding without class

//basically closures were private state before classes existed
```

