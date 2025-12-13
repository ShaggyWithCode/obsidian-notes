
- Prevent unnecessary re-rendering in child component by saving function definition
- *The function we talking about is callback function* , reason is we pass callback function as parameter to other function
- it is crucial that child component wrapped in `React.memo` to avoid re-render
- functions are object and getting recreated when component re-renders 
- when child component is wrapped in `React.memo` it compare function with cached function when it first rendered, if function is same as before then it don't allow it to re-render
- when we use `useCallback` , it return stable reference like it had before based on dependency array. i.e if dependency array changed == reference change 

```
	const memoiseFun = useCallback(
	
		ourFunction();
	, 
	[dependency1, dependency2]
	)
```

Example :
```
const ListItem = React.memo(({ item, onToggle }) => 
	{  return 
		( 
		<div style={{ padding: '5px', borderBottom: '1px solid #eee' }}> 
			<span>{item.name} </span> 
			<button onClick={() => onToggle(item.id)} > Toggle Status </button>  
			<span > {item.active ? 'Active' : 'Inactive'} </span> </div> ); 
	}
); 
			
export default ListItem;


import React, { useState, useCallback } from 'react';
import ListItem from './ListItem'; 
const initialList = [ { id: 1, name: 'Task A', active: false }, { id: 2, name: 'Task B', active: true }, { id: 3, name: 'Task C', active: false }, ]; 

function OptimizedList() { 
	const [list, setList] = useState(initialList); 
	const [theme, setTheme] = useState('light'); 
	const handleToggle = useCallback((id) => {  
		setList(prevList => 
			prevList.map(item => item.id === id ? 
				{ ...item, active: !item.active } 
				: item ) 
			); 
	}, []);
	
	return ( 
		<div> 
			<h1>Optimized List</h1>
			<button 
				onClick={() => setTheme(theme === 'light' ? 'dark' : 'light')}> 
				Toggle Theme (Triggers Parent Render) 
			</button>
			 <hr /> 
			 <div> 
				 {list.map(item => 
					 (
					 <ListItem key={item.id} item={item} onToggle={handleToggle}/>                      )
				)
				}
					  </div> </div> ); 
	}
```

- Few Points to remember, the dependencies are mostly `sideeffect` related dependencies 
- We should use functional update of useState setter as it avoid lot of functional recreation overhead 
	- Note -  when we pass state in dependencies, function will recreate every time whenever that state changes, its recreational overhead, in that case we should use functional state update which allow us not to pass state in dependency which will end up saving many `rerenders` 
