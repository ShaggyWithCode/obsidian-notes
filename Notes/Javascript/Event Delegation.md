
=> Its a technique where you attach one event listener to parent element to handle events for multiple child elements, why ? because of event bubbling - Events travel `Up` the DOM tree.

### **Q: When would you use event delegation?**

**Answer:** "I use event delegation when I have many similar elements (like list items) or elements that are dynamically added/removed. It reduces memory usage and simplifies code. I also use it for generic handling like form validation or menu navigation."

### **Q: What are the limitations of event delegation?**

**Answer:** "It only works for events that bubble. For non-bubbling events like `focus`, I use `focusin` or capture phase. Also, I must be careful with `event.target` when there are nested elements - that's why I use `closest()` to find the intended parent. Another limitation: if any handler calls `stopPropagation()`, delegation breaks."

### **Q: How do you handle events on elements that are removed and re-added?**

**Answer:** "With event delegation, I don't need to reattach listeners - the parent listener stays, so any new children automatically work. That's one of the main benefits."

### **Q: Does React use event delegation?**

**Answer:** "Yes, React uses a single event listener on the root for all events and delegates internally through its synthetic event system. This is one reason why React events work consistently across browsers."

### **Q: Can event delegation cause performance issues?**

**Answer:** "Rarely. A single listener is almost always faster than many. But if the parent handles many different types of events and does heavy filtering, it might cause slight overhead. In practice, it's negligible unless you're processing hundreds of events per second (like mousemove)."

### **Q: How do you implement event delegation for custom events?**

**Answer:** "Custom events bubble by default if `bubbles: true` is set when dispatching. So you can listen on a parent just like native events."

---

## **Practical Example: Todo List with Actions**

html

```
<div id="todo-app">
  <ul id="todo-list">
    <li>
      <span>Buy milk</span>
      <button class="delete">X</button>
      <button class="edit">✎</button>
    </li>
    <!-- more items -->
  </ul>
  <button id="add">Add Item</button>
</div>
```

javascript - 

```
document.getElementById('todo-app').addEventListener('click', (e) => {
  const target = e.target;
  
  // Handle delete button
  if (target.matches('.delete')) {
    const li = target.closest('li');
    li.remove();
  }
  
  // Handle edit button
  else if (target.matches('.edit')) {
    const li = target.closest('li');
    const textSpan = li.querySelector('span');
    const newText = prompt('Edit item:', textSpan.textContent);
    if (newText) textSpan.textContent = newText;
  }
  
  // Handle add button
  else if (target.matches('#add')) {
    const text = prompt('New item:');
    if (text) {
      const li = document.createElement('li');
      li.innerHTML = `<span>${text}</span>
                      <button class="delete">X</button>
                      <button class="edit">✎</button>`;
      document.getElementById('todo-list').appendChild(li);
    }
  }
});
```

---

## **Summary**

- **Event delegation** = one parent listener for multiple children
    
- Relies on **event bubbling**
    
- Benefits: **performance**, **dynamic elements**, **clean code**
    
- Use `event.target` and `closest()` to identify the actual element
    
- Be aware of **non-bubbling events** (use alternatives)
    
- Don't call `stopPropagation()` unnecessarily
    
- Widely used in frameworks
    

**Interview ready:** Understand bubbling, target checking, and when to apply it. Know the trade-offs.