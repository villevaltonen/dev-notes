## Dynamic properties

```javascript
const handleChange = (e) => {
  const name = e.target.name;
  const value = e.target.value;
  setPerson({ ...person, [name]: value });
};
```
