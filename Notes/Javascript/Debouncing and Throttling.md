

### **Q1: Explain debouncing and throttling with examples.**

**Answer:** "Debouncing ensures a function runs only after a pause in calls – like waiting for the user to stop typing before making a search API request. 
Throttling ensures a function runs at most once per time interval Ex. When you scroll, the browser tries to run your code 100 times a second; throttling forces it to "wait" 200ms between every successful function execution.\. 
Both improve performance by reducing the number of times a function executes."

### **Q2: When would you use debounce vs throttle?**

**Answer:** "I use debounce when I care about the final state after a burst of events, e.g., search input, auto-save, resize end. 
I use throttle when I need periodic updates during the burst, e.g., scroll-based animations, tracking mouse movement, or preventing excessive API calls while still showing progress."

### **Q3: How would you implement a simple debounce function?**

**Answer:** "I'd create a function that takes a callback and a delay. It returns a new function that, when called, clears any existing timer and sets a new one. When the timer expires, it calls the callback with the original `this` and arguments using `apply`."

### **Q4: How do you handle the leading edge (immediate execution) in debounce?**

**Answer:** "I'd add a flag to track if the function should run immediately. For example, if `leading` is true and no timer is pending, run immediately and then set a timer to reset the 'ready' state after the delay. Lodash's debounce implements both leading and trailing options."

### **Q5: Can debounce or throttle cause memory leaks?**

**Answer:** "If timers are not cleared when a component unmounts (in a UI framework), they can hold references to the component's scope and prevent garbage collection. That's why it's important to provide a `cancel` method and call it in cleanup."





`Debouncing and throttling` are the techniques to `control` how `often function should run`, especially `for events that fire rapidly`. 

ex. 
```
Imagine your app has a search box that fetches results as you type. If you send a request on every keystroke, you might:

- Overwhelm the server with too many requests
    
- Waste bandwidth
    
- Cause UI lag
```

### Debouncing - wait for pause
Debouncing ensure that function is **called only after certain amount of time `passed`** since the last time it was called. 

#### how it works
1. function called (e.g. , user enter 'a' )
2. timer starts to delay (e.g. , say 300s)
3. if function called again before timer end, reset timer
4. Timer finally Expire -> execute function 

#### Real-world use case
- Search Input
- Form Validation
- Resize event

**Implementation** 
```
function debounce(func, delay) {
  let timeoutId;
  
  return function(...args) {
    // Clear previous timer
    clearTimeout(timeoutId);
    
    // Set new timer
    timeoutId = setTimeout(() => {
      func.apply(this, args);  // using this as if function is coming from obj
    }, delay);
  };
}

// Usage
const searchInput = document.getElementById('search');
const debouncedSearch = debounce((event) => {
  console.log('Searching for:', event.target.value);
  // API call...
}, 300);

searchInput.addEventListener('input', debouncedSearch);
```


### **Leading vs Trailing (Edge cases)**

by default debounce executes after a pause  (trailing case), sometime we want to execute it early than wait

`lodash's` debounce offers `leading` and `training` options

`Leading` - call function before beginning of burst
`trailing` - call after pause

```

// Example: Button to prevent double-click
button.addEventListener('click', debounce(() => {
  console.log('Clicked!'); // Only once even if double-clicked
}, 300, { leading: true, trailing: false }));

```

## **Throttling – At Most Once Per Interval**

Function is called at most `once in given Interval of time`

How it works
1. function called
2. check enough time passed since last execution
3. if yes, the execute and record time
4. if not, ignore or schedule for latter 

Real-world use case
- Scroll event (update sticky navigation, lazy load images)
- Resize event (redraw canvas)
- Mouse move (tracking position, but not too often)
- Button clicks (prevent double submission, though debounce often used)

## **Real-World Usage Scenarios (Senior Insight)**

### **Debounce**

- **Auto-save** in an editor: save only after user stops typing
- **Typeahead suggestions**: avoid API calls on every keystroke
- **Form validation**: validate after user finishes input
- **Window resize**: recalculate layout after user finishes resizing

### **Throttle**

- **Scroll-based animations**: update element position but not every pixel
- **Lazy loading images**: check which images are visible, but not every scroll event
- **Game loop**: limit updates to 60fps even if device can handle more
- **Analytics tracking**: send events but not more than once per second

### **Cancellation**

Sometimes you need to cancel a debounced call (e.g., component unmounts). Good implementations return a function with a `cancel` method.

```
function debounce(func, delay) {
  let timeoutId;
  
  const debounced = function(...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => func.apply(this, args), delay);
  };
  
  debounced.cancel = () => clearTimeout(timeoutId);
  
  return debounced;
}
// Usage
const debouncedFn = debounce(() => {}, 300);
// later...
debouncedFn.cancel();

```


### **Immediate Invocation with Debounce (Leading)**

If you want the function to run immediately and then ignore subsequent calls for a delay (like a "throttle" but resetting timer), use leading debounce.

```
// Lodash style
const fn = debounce(myFunc, 300, { leading: true, trailing: false });
```


### **Combining with Promises / Async**

Be careful when debouncing async functions. If the function returns a promise, the caller might expect it to resolve. Typically debounce wraps the inner function, and the outer function returns `undefined` or the last promise.


### *Important Mistake*

1. **Creating new debounced function each render** (in React): This breaks debouncing because the function identity changes. Use `useCallback` or `useRef` to keep the same instance.
2. **Forgetting to cancel on unmount** (React): If component unmounts before debounced function runs, it might cause memory leaks or state updates on unmounted component.
3. **Not preserving `this` context**: Always use `func.apply(this, args)` in the wrapper, or arrow functions that capture correct `this`.


## **Summary**

- **Debouncing** – group a burst of events into a single call after a pause.
- **Throttling** – ensure a function is called at most once per time period.
- Both are essential for performance in event-heavy applications.
- Choose based on whether you need the final state (debounce) or periodic updates (throttle).
- Always handle `this` and arguments correctly, and provide cancellation for cleanup.