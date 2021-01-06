## Events

```javascript
const button = document.querySelector('.btn');
// click, mouseover, mouseout, input etc.
button.addEventListener('click', (e) => {
    e.preventDefault();
    console.log('click');
})
```