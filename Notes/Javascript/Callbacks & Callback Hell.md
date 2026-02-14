- a callback is a function you give to another function to be called letter.
```
setTimeout(() => {
  console.log("Hello");
}, 1000);

here () => { console.log("Hello") } is a callback
```

- **why callback needed ?** - JavaScript is single treaded and non-blocking So instead of waiting JavaScript say i will call you back when im done. 
- but when we use so many nested callbacks for sequential tasks , it leads to  `callback hell`
```
// Example of Callback Hell
getData(function(a) {
    getMoreData(a, function(b) {
        getEvenMoreData(b, function(c) {
            console.log(c);
        });
    });
});
```
- like in this given example, it became hard to read, hard to debug, error handling become painful and code grows at right side direction. to solve that ,we use promise .

- **Promises** - fixing callback hell
- promise is a object which represent a `value`(i.e. Promise{ <`pending`>} or Promise{ <`fulfilled` : value>} or rejected with error as well)  which will `available later` . it has three state - `pending, fulfilled,  rejected`.
```

//## Creating a promise
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Success");
  }, 1000);
});
```
- now lets consider every function above returning a promise so this is how we handle it -
```
getData()
	.then((a) => getMoreData(a))
	.then(b = > getEvenMoreData(b))
	.then((c)=> console.log(c))
	.catch(err => console.log(err))
```

- **Async/Await** : async await is just a synthetic sugar over a promises.
```
async function processData(a) {
	try{
		const a = await getData()
		const b = await getMoreData()
		const c = await getEvenMoreData()
		console.log(c)
	}catch(e){
		console.log(e)
	}

}
```


- here we need to keep in mind that its function arrangement we do. i.e. passing callback function, or make function into promise . so here the function `getData()` will be having promise implementation inside. 