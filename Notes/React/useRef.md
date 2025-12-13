- useRef hook returns a mutable object whose `.current` property can be initialized to value passed to it through argument.
- this object persists for all lifecycle of component.
- Means Changing the `.current` property will not re-render the component . 
- that make it bit different than declarative way of react.

**Usages**
1. DOM Interaction
	 - Since React manages the component tree, if we need to do direct browser action on element like focus, playing video or measure size, we need to have reference to DOM node. useRef help us for that.   
	  Ex.
```
			const buttonRef = useRef(null)
			const focus = () => inputRef.current && {buttonRef.current.focus()
			return (
				<>
					<input ref=`{buttonRef}` />
				<button onClick = {()=>focus()} />
					</>
				)	
```
		

	
- This is a last resort as if we can do it without prop/state change 

2. **Persistent Storage**
	 - it can store value which will persistent acress render (like instance value)
	 - and will not trigger Rerender on change

3. Capture Previous State
	 - Its a custom hook when useEffect used to store current value late while we return useRef value early

```
const usePrevious = (value) => 
	{ 
	const ref = useRef(); // The persistent storage box // This runs AFTER the component has rendered. 
	useEffect(() => 
			{ // The current 'value' is saved here for the *next* render cycle.                    ref.current = value; 
			}); // The function returns the value saved during the LAST render.        return ref.current; 
	};
```

Tip - useEffect run after commit phase(react has rendering(planning), commit phases), so this hook return value of ref.current first and then assign value to ref.current after it got committed on screen. 