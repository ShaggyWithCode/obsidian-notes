
rendering meaning conversion of JSX into actual UI element(HTML)

meaning jsx is actual HTML 
what happens is JSX get compiled(using babel) in covert into React.Element calls, these calls create virtual DOM object and then it convert in HTML using renderer(ReactDOM)

Component (JSX)
      ↓ (using babel while compilation)
React.createElement()
      ↓ (this createElement function call get called results into virtual DOM)
Virtual DOM (JS objects)
      ↓
React Reconciler (diffing, figuring out changes)
      ↓
React Renderer (ReactDOM)
      ↓ (getting converted into real DOM)
Browser’s Real DOM (via document.createElement, setAttribute, etc.)


react component [[re-render]] when state of prop changes .



