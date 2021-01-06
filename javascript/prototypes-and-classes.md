### OOP: Prototypes and classes

Class (ES5)
```javascript
// Constructor function
function Person(firstName, lastName, dob) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.dob = new Date(dob);
    this.getBirthYear = function() {
        return this.dob.getFullYear();
    }
}

// Instantiate object
const person1 = new Person('John', 'Doe', '1-1-1980');
```

Prototype (ES5)
```javascript
// This way methods won't be in the object, they'll be in the prototype
Person.prototype.getBirthYear = function() {
    return this.dob.getFullYear();
}
```

Class (ES6)
```javascript
class Person {
    constructor(firstName, lastName, dob) {
        this.firstName = firstName;
        this.lastName = lastName;
        this.dob = new Date(dob);
    }

    getBirthYear() {
        return this.dob.getFullYear();
    }
}
```