1. `Object.assign()` - it does `shallow` copy **only**
	- it copies properties from source to target i.e `object.assign(target,...source)`
	- as its shallow copy, it copies only the first level , Nested objects are shared 
			const obj = { nested: { x: 1 } };
			const copy = Object.assign({}, obj);
			copy.nested.x = 99;
			console.log(obj.nested.x); // 99 😬
2. `Object.keys()`  - returns array of objects own enumerable(visible props in object // not props like __proto__) property names . and its first level only props.
```
	const user = {
		  name: "Sagar",
		  address: {
		    city: "Pune",
			pin: 411001
		  },
		  skills: {
		    primary: "JS",
		    secondary: "Go"
		  }
	};
	
	Object.keys(user) // ["name", "address", "skills"]
```
3. `Object.entries()` : perfect for iteration . gives key value pair
```
	const user = {
		  name: "Sagar",
		  address: {
		    city: "Pune",
			pin: 411001
		  },
		  skills: {
		    primary: "JS",
		    secondary: "Go"
		  }
	};
	
	Object.entries(user) // 
	output => [["name","Sagar"], ["address",{city: "Pune",pin: 411001}], ["skills",{primary: "JS",secondary: "Go"}]]
```
 
