## Fetch API

- a way to make http-requests from browser
- asynchronous

```javascript
// The first parameter is URL
// The second one is optional and is used to configure the request: methods, headers etc.
// Without the second one, default is GET-request
fetch('http://localhost:8080/api/getsomething', {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json'
    }
    body: JSON.stringify({
        name: 'User 1'
    })
}) // Return Promise
    .then(res => {
        if(res.ok) {
            return res.json())
        } else {
            console.log("FAILURE")
        }
    }
    .then(data => console.log(data))
    
```