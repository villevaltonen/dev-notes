## Object literals

```javascript
const person = {
    firstName: 'John',
    lastName: 'Doe',
    age: 30,
    hobbies: ['music', 'movies']
    address: {
        street: '50 main st',
        city: 'Boston',
        state: 'NA'
    }
}

const { firstName, lastName, address: { city }} = person;

```