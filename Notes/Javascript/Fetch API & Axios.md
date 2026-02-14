**Fetch API**
```
fetch("/api/users")
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.error(err));
```
### Important Fetch caveat ❗

```
fetch("/404")
   .then(res => {
        if (!res.ok) throw new Error("HTTP error");
             return res.json();
        }
    );`
```

❌ Fetch does NOT reject on HTTP errors  
✔ It rejects only on network failure
- res => res.json() part is there

## Fetch with async/await (recommended)

```
async function loadUsers() {
   const res = await fetch("/api/users");
      if (!res.ok) throw new Error("Failed");
        const data = await res.json();
	    return data;
 }
 ```


## Axios (popular library)

`import axios from "axios";
`axios.get("/api/users")
  `.then(res => console.log(res.data))
   `.catch(err => console.error(err)
`);`

### Why people use Axios

- Auto JSON parsing (No res => res.json() like in fetch)
    
- Rejects on HTTP errors
    
- Interceptors
    
- Request/response transforms


---

## Axios with async/await

```
async function loadUsers(){
	const res = await axios.get("/api/users");
	return res.data;
	}
```

Cleaner, fewer checks.