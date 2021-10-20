## Memoization

- An efficient way to reduce the time and space complexity of recursive calls
- In short, storing the results from recursive calls to a hashmap or similar, so any identical recursive calls can be exited right away and use the pre-calculated value with the same key from the map

```javascript
const fib = (n, memo = {}) => {
    if(n in memo) return memo[n];
    if(n <= 2) return 1;
    memo[n] = fib(n - 1) + fib(n - 2);
    return memo[n];
}
```