ES Modules replaced `CommonJS` `require` with standardized module system
these imports must be at top

1.Named Exports:  `export const myVar = 1`
2.Default export: `export default myfun`

import 
```

import myFun, {myVar} from "file.js";
```

some bundles like vite, webpack looks at your import statement try to remove unused code .
- when we use default import(ex. onject of multiple functions) , whatever comes with default import need to keep in bundle
- when used named then that only part is used and other can be removed . Hence for optimize always used named import. not default. 
