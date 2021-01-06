## Component

- Returns JSX

Creating a component

```javascript
// Regular
function Greeting() {
  return <h4>This is my first component</h4>;
}

// Arrow function
```

Injecting React into index.html

```javascript
ReactDom.render(<Greeting />, document.getElementById("root"));
```
