
- Prevents resource-intensive calculations running during every component re-render
- `memoize` result of function execution . and allow the function rerun when some input(dependencies) change


```
const usedMemoFun = useMemo(
	() =>{
		return letCalculate(a,b)
	},
		[a, b]
	)
```

- 1. Usecase - we should use it to caching Expensive values - to avoid wastage of cpu cycles for complex calculations. 
- 2.Usecase - Stabilizing object array references 
	 - when we pass object to component as a prop, on every render the new reference created for it, so in case we are passing that to component wrappen in React.memo, it will re-render that component. that is why we use useMemo for those objects.