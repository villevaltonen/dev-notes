## Variables and data types

### Variables
- three ways
    - var (old syntax, should not be used anymore, due global scope)
    - let 
        - can be re-assigned
    - const
        - constant, can not be re-assigned

### Data types
String
```javascript
const name = 'John';

// Concatenation
'John ' + 'Wick'

// Template String
`My name is ${name}`

// String methods
const s = 'Hello World';
s.length;
s.substring(0, 3);
s.split(',');
```

Numbers
```javascript
const age = 30;
const rating = 4.5;
```

Boolean
```javascript
const isCool = true;
```

null
```javascript
const x = null;
```

undefined
```javascript
const y = undefined;
let z;
```

Checking type of variable
```javascript
console.log(typeof name);
```