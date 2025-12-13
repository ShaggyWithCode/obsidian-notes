
1.**When state changes** (via useState, setState )
-When state changes, that component marked dirty.
-React Schedule re-render on Dirty component
-It call that component again and create Virtual DOM for it and do reconciliation . and update only changed part.

2.**When prop change**(from parent -> child)
-Unless it wrapped with React.memo

3.**When Parent Re-renders**
-It will re-render child by default
-Here also we can wrap child with React.memo to avoid rerendering

4.When Context value change
-Any change in Context's **value** re renders components consuming it