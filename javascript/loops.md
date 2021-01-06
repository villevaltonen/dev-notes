## Loops

### For-loop
```javascript
for(let i = 0; i < 10; i++) {
    console.log(i);
}
```

### While-loop
```javascript
let i = 0;
while(i < 10) {
    console.log(i);
    i++;
}
```

### Looping through an array
```javascript
const todos = [
    {
        id: 1,
        text: 'foo'
    },
    {
        id: 2,
        text: 'bar'
    }
]

// Naive way of looping through
for(let todo from todos) {
    console.log(todo.id);
}

// forEach
todos.forEach(function(todo) {
    console.log(todo.text);
});

// map
const todoText = todos.map(function(todo) {
    return todo.text;
});

// filter
const todoIds = todos.map(function(todo) {
    return todo.id === 1;
});
```