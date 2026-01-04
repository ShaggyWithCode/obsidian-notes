
- in classic inheritance hierarchy is fixed, in prototypical its flexible (since its refer other objects prototype)
- in classical , object defines the behavior (i.e house class has info of how that be build) . but in prototypical Behavior is shared by prototype .
- example of prototypical inheritance (llok look how it does without class)-
```
const animal = function (){
	speak(){
		return "sound"
	}
}

const dog = Object.create(animal)
dog.bark = () => "woof"
dog.speak() //sound
dog.bark() // woof
```

- basically, in js , inheritance is delegation, not copying
- When you call `user.sayHello()`, JavaScript looks at the `user` object. If it’s not there, it looks at `user.__proto__`. If not there, it looks at `user.__proto__.__proto__`, and so on, until it hits `Object.prototype` or `null`. This is **delegation**, not copying.
