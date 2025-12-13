//why hooks replaced class components 

if you are using hooks, that means your function is [[stateful]], else its [[stateless]]. Before hooks stateful component was created by Class component which has state prop.

We use **hooks** to make components **dynamic** and **interactive**. We should mind a term of “remember data” (state) as this data supposed to update afterwards which afterwards update/re-render the component.

Hooks are functions that let you use React **state** and **lifecycle** features (like managing side effects) within functional components.

these are hooks -
core hooks - [[useState]], [[useEffect]], [[useContext]], [[useRef]]
performance hooks -  [[useCallback]] (React.memo), [[useMemo]]  , [[useReducer]] 

we can create [[custom hooks]] as well .