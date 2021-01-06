## Window-object

- window-object is a top level object in the browser, which contains things like:
    - fetch API
    - document (HTML etc.)
    - etc...
- methods of the window-object can be called without specifying the object
```javascript
document.getElementById('foo');
// is the same as
window.document.getElementById('foo');
```