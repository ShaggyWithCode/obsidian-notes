```
class Db {
	constructor(){
		this.name = "pikchu"
	}
	
	connect(){
		console.log(`${this.name}`)
	}
	
	static getVersion(){
		return `1.2.3`
	}
}

here we can- 

const db = new Db

db.connect() //will work
db.getVersion() //will not work
Db.getVersion() //will work
```

- Static methods **belong to the class**, not to objects created from it.
- `this` in static is belong in class itself, not in instance. [[this is static usage]] 