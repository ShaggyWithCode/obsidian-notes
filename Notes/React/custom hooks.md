- custom hook also a important design pattern
- we not only use it for "reusing code" but also use it for abstracting logic, separating concerns and creating clean and declarative component `Api`
- the goal is to make component stupid as its job should be rendering the `jsx` only.it should not be cluttered with complex `useEffect`, data transformations or state management . all of that `Thinking` should be moved into custom Hook.


Pattern 1: The Logic Encapsulation
- in this pattern complex level of state and state related functions is encapsulated into a hook. 
- this called as headless hook as it provide logic without any of UI.
- ex.

```
export function useToggle(intialState = false){
	const [isOn, setIsOn] = useState(intialState);
	
	const toggle = () => setIsOn((prev) => !prev)
	const setOn = () => setIsOn(true)
	const setOff = () => setIsOn(false)
	
	//for function stability we should wrap the code in useCallback
	//as its related state, dependency array would be empty 
	
	return {isOn, toggle, setOn, setOff}
}
```

cleaned component code ->

```
function myComponent({toggle}){
	const {isOn, toggle, setOn, setOff} = useToggle(toggle)
	.
	remaining logic
	.
	.
	.
	.
}
```


Pattern 2:The Lifecycle abstractor (Taming `useEffect`)
- `useEffect` hook logic is always bit messy. and there are always chances to forget dependency array and cleanup function.
-  is custom hook we wrap all this logic
- ex. fetch Api with load,error with race condition and cleanup

ex:
```
function useFetchHook = () => {
	const controller = new AbortController();
	const [state, setState] = useState({ data = null, error: null, loading: true})
	
	useEffect( () => {
		const controller = new AbortController();
		
		setState({ data = null, error: null, loading: true})
		
		fetch(url, { signal: constoller.signal})
			.then((response) => response.json())
			.then((data) => setData({ data = data, error: null, loading: true}))
			.catch((error)=> setData({ data = null, error, loading: true}));
			
		return () => controller.abort();
		}, [ url ]
	); 
	return state;
}
```

Pattern 3: Parameterized Behavior (Behavior Injection) :   In this way, hooks receive callback or config to create reusable and dynamic logic.
ex. `useForm` with validation logic passed in

```
export function useForm(initialValues, validateFn) {
  const [values, setValues] = useState(initialValues);
  const [errors, setErrors] = useState({});

  const handleChange = e => {
    const { name, value } = e.target;
    setValues(prev => ({ ...prev, [name]: value }));
  };

  const handleSubmit = callback => {
    const validationErrors = validateFn(values);
    setErrors(validationErrors);

    if (Object.keys(validationErrors).length === 0) {
      callback(values);
    }
  };

  return { values, errors, handleChange, handleSubmit };
}

```
