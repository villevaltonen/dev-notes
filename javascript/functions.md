## Functions

```javascript
function addNums(num1, num2) {
    console.log(num1 + num2);
}

addNums(5, 4); // 9
addNums(); // NaN = not a number

function addNums(num = 1, num2 =2) { // Default values
    ...
}
```

Arrow functions
```javascript
const addNums = (num1, num2) => {
    return num1 + num2;
}

// Simple syntax
const addNums = (num1, num2) => num1 + num2;

// With forEach etc.
todos.forEach((todo) => console.log(todo));
```
