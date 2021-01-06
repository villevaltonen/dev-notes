## Arrays

```javascript
// Initialization
const fruits = ['apples', 'oranges', 'pears', 10, true];

// Accessing elements
fruits[1] = 'foo';
let foo = fruits[1];

// Adding to an array
fruits.push('mangos');
fruist.unshift('strawberries');

// Removing the last element
fruits.pop('')

// Check index
fruits.indexOf('oranges');
```

### Array of objects
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

let foo = todos[1].text;
```