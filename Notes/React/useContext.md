- ideal tool to avoid prop Drilling
-code EX.
we use createContext, useContext from react to use Context
1. create createContext to create Context
	ex. ThemeContext = createContext(defaultValue)
2. wrap component with provider of created context and pass props/values through  values
	 ex.  <ThemeContext.Provider value= {props}>
		 <component />
		<ThemeContext.Provider />
3. in component access that contaxt with useContext hook
4. Masters use it for global , App wide state, or theme, Authentication, localization
5. Masters also use it for mini redux pattern i.e use useContext and useReducer togather
6. Scoped state i.e if one parent has multiple child ex. form which made of many inputs component, we can wrap that form parent component with provider