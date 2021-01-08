## Context

- Helps to reduce "prop drilling"
- Creates a way to access, e.g. functions, inside the root component within the child components
- Not a replacement for Redux though

```javascript
import React {useState, useContext} from 'react';

const PersonContext = React.createContext();
// two components - Provider, Consumer

// Root component
const Foo = () => {
    // ...

    return (
        <PersonContext.Provider value={{removePerson}}>
        <h3>Foobar</h3>
        </PersonContext.Provider>
    )
};

// Child component
const Bar = () => {
    // ...
    const {removePerson} = useContext(PersonContext);
}
```
