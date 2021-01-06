## Async and await

### Async functions
- async means that a function always returns a promise
- other values are wrapped in a resolved promise automatically
```javascript
async function f() {
    return 1; // Same as return Promise.resolve(1);
}
f.then(alert);
```

### Await
- suspends the function execution until promise settles so JS engine can keep doing other things meanwhile
```javascript
// works only inside async functions
let value = await promise;

// For example
async function f() {
    let promise = new Promise((resolve, reject) => {
        setTimeout(() => resolve("done!"), 1000)
    });
    let result = await promise;
    alert(result);
}
f();
```

- await does not work in top-level code, but it can be wrapped into an anonymous function
```javascript
(async () => {
    let foo = await fetch('/api/here.json');
    let user = await foo.json();
})
```

- await accepts "thenables" (objects wit a callable then method)
- await can be catched like "a throw" (throw new Error('Whoops!')) with try-catch