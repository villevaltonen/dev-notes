## Component

- Returns JSX always
- Returns only a single element (JSX-rule)
- In JSX:
  - use className instead of class
  - closing tags are required always
- Wrapping JSX into parentheses (); helps you keep the code more readable

Creating a components

```javascript
// Stateless functional component
// With regular syntax
function Greeting() {
  return <h4>This is my first component</h4>;
}

// With an arrow function
const Greeting = () => {
  return <h4>This is my first component</h4>;
};
```

Injecting React into index.html

```javascript
ReactDom.render(<Greeting />, document.getElementById("root"));
```
