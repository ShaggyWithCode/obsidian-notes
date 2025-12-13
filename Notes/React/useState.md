

- component specific
- for independent component
- for form input, counters, show hide logic

	ex. const [ getVal, setVal ] = useState ( defaultval )

- **Lazy initialization** - since useState runs on every render if we have any expansive function call to make, we don't call it directly while initialization of useState
		i.e const [get, set] = useState(expansiveFunc())
  instead, we call it by passing function into initialization 
		i.e const [get, set] = useState(***()=>{expansiveFunc()*}**)
  this pattern is known as Lazy initialization as expansiveFunc() will run only at first render i.e on initial mount . 
- In short passing function to useState ensures that Initialization logic will execute once only during components initial render.
- use it for synchronous ops like  reading from local storage , complex object construction 


- **Function Update** - function in setter guarantees the latest state
     - when we pass function to setter, react pauses the state update, goes into queue and fetch current/fresh state and pass it through function argument  i.e prev
     ex. setVal( ( prev ) => { prev + 1 })
     
- **State Batching**
	 - It means grouping multiple state updates into one render cycle
	 - it boosts performance, avoid flickering
	 - for ex. if incode we have setName('abc'),setNamevalue('123') -> these are 2 state updates but it will not make rerender 2 times, it will happen once. 
	 - In past batching was there for browser events(Ex. click and others) and NOT for async code(this code is not in reacts direct control as its not known hold it should it execute) but after React 18 its almost for all. 

                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            

**Now with all this info we should understand why we write counter by passing function in setter**