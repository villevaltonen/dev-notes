## Conditionals

### If-else
```javascript
const x = 10; 

// double equal matches only values
if(x == '10') {
    console.log('x is 10'); // true
}

// triple equal matches values and the data type
if(x === '10') {
    console.log('x is 10'); // false
})

// if-else
if(x === 10) {
    console.log('it is');
} else {
    console.log('it is not');
}

// ternary operator
const color = x > 10 ? 'red' : 'blue';
```

### Switch
```javascript
const x = 11;

switch(color) {
    case 'red':
        console.log('color is red');
        break;
    case 'blue'):
        console.log('color is blue');
        break;
    default:
        console.log('color is not red or blue');
}
```