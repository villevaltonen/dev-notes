## DOM

### Selection
```javascript
// Single element
document.getElementById('my-form');
document.querySelector('my-form'); // Introduced later

// Multiple elements
document.querySelectorAll('h1'); // Returns a NodeList, array-methods can be used
dockument.getElementsByClassName('h1'); // Returns a HTMLCollection
```

### Manipulating
```javascript
// Manipulating the DOM
const ul = document.querySelector('li');
ul.firstElementChild.textContent = 'Hello';
ul.children[1].innerText = 'Brad';
ul.lastElementChild.innerHTML = '<h1>Hello</h1>';

// Changing style
ul.style.background = 'red';
```