1. **`Destructuring` ({}/[]) - extract/unpack values**
```
		###Destructuring of object and array
		const user = { name: "Sagar", age: 30 };
		
		const { name, age } = user;
		
		const [a, b] = [10, 20];
		
		
		
		### Default values (important)

		const { role = "user" } = {};

```

2. **Spread (`...`)** = **expand**
	- by using spread we can copy object, but that's lead to shallow copy only 
	- it breaks reference at top level only (example 2)
	ex.
```
Example 1 - 

const a = [1,2]
const b = [...a,3] // [1,2,3]

const aob= {a : 1, b: 2}
const b = {...aob, c: 3} // {a:1, b:2, c:3}

-------------------------------------------------------------------------
Example 2 - 
	const original = {
		  name: "Sagar",
		  address: {
		    city: "Mumbai"
		  }
	};

	const copy = { ...original }; //this is how we copy using spread operator


Memory picture:

Copy code
original ──┐
           ├── address ──► { city: "Mumbai" }
copy     ──┘


Result
copy.address.city = "Pune";

console.log(original.address.city); // "Pune" ❗
```

3. **Rest (`...`) = collect**
```
function sum(...nums) {
  return nums.reduce((a, b) => a + b);
}
```


Spread => outword
rest => inword