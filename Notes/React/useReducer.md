- Its alternate to `useState` but for complex state where state is dependent on previous state and the state logic needs to be extracted outside of component. 
- in `useState` we have `[getState,setState]`, in `useReducer` we have `[state, dispatch]`
- the initial state we pass to `useReducer` function is a state which we get from Reducer which we have written externally.
- it avoid stale closures (outdated state) as well.

ex. 

```
const countReducer = (state, action)=>{
     switch(action.type){
          case "increase":
             return {count : state.count + 1}
           case "decrease":
             return {count : state.count - 1}
          case "reset":
             return {count : 0}
         default:
	         return state
     }
}
```

```
function Component(){
    const[state, dispatch] = useReducer(countReducer, {count:0})
     const increase = () => {
         dispatch({type:"increase"})
     }
}
```

- As application grows, we can separate complex state and add `useState`  related logic grouping into `useReducer` 
