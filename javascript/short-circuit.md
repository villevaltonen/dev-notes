## Short circuit

```javascript
const [text, setText] = useState("hello world");
const firstValue = text || "hello world";
const secondValue = text && "hello world"; //

console.log(firstValue); // empty
console.log(secondValue); // hello world

setText("");
console.log(firstValue); // hello world
console.log(secondValue); // empty
```
