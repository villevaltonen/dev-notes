## Performance optimization

- React is fast by default

### React.memo

- A function, to which your component properties are passed to
- If property does not change, there is no need to re-render

```javascript
const BigList = React.memo({products}) => {
    // ...
}
```

### useCallback

- Similar to React.memo, but for a function

```javascript
const addToCart = useCallback(() => {
  setCart(cart + 1);
}, [cart]);
```

### useMemo

- Remembers the value

```javascript
const mostExpensive = useMemo(() => calculateMostExpensive(products), [
  products,
]);
```
