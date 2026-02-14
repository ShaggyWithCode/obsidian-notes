- used for string interpolation, multiline strings and embedded expressions
Ex.
```

const name = "Sagar";
console.log(`my name ${name}`) //my name sagar

//Expression

`${count > 0 ? "yes":"no"}` 

//Tagged templates
tag`Hello ${name}` //this fumctions need to be write before to call like this

```

- Avoid nested ternary operations in template literals 