- HOF are functions which either takes one or more functions as arguments or return function as a result
- map, filter & reduce

- **map**
	- returns array of same length 
	- transform values
```

const arr = [1,2,3]

const mapOut = arr.map(arrItem => arrItem * 2 ) //2,4,6
```

- **Filter**
	- selects values
	- length may change 

```
const arr = [1,2,3]

const mapOut = arr.map(arrItem => arrItem % 2 === 0 ) //2
```

- **Reduce**
	- used for sums, grouping, flattering and state machines 