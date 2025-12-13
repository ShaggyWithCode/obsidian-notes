
- this hook enable us to lifecycle features and Side effect (outside world operations)
-ex . useEffect( () => {} , [ ] )
- empty [ ] means effect runs only once after initial render
- [ variable ] means effect runs when variable changes
- we can return a function which will act as cleanup
-ex. 
useEffect( () => {
var isCurrent = true

fetch('url').then((response)=>{ if (isCurrent){ // write response}})
 return () =>{
 //cleanup code here
 isCurrent = false
 }
} , [ userId ] )

