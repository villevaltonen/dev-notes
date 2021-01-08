## Hooks

### useState

- the hook method has to be imported from react-package
- useState returns an array, where first element is the value and the second one is the handler
- components which use hooks, have to start with upper case
- must be in the function/component body
- remember to use an arrow function e.g. in button for events, because otherwise the function will be invoked as soon as component renders, not on click or whatever event is used

```javascript
// "random text" is the default value
import React, { useState } from "react";

const UseStateBasics = () => {
  const [text, setText] = useState("random text");

  const handleClick = () => {
    setText("hello world");
  };

  return (
    <React.Fragment>
      <h1>{text}</h1>
      <button className="btn" onClick={handleClick}>
        Change title
      </button>
    </React.Fragment>
  );
};
export default UseStateBasics;

// useState with an array
const data = [
  {
    id: 1,
    name: "John",
  },
  {
    id: 2,
    name: "Suz",
  },
];

const UseStateArray = () => {
  const [people, setPeople] = useState(data);
  return (
    <>
      {people.map((person) => {
        const { name, id } = person;
        return (
          <div key={id} className="item">
            <h4>{name}</h4>
          </div>
        );
      })}
      <button className="btn" onClick={() => setPeople([])}>
        Clear
      </button>
    </>
  );
};

export default UseStateArray;

// useState with an object
const [person, setPerson] = useStaet({
  name: "john",
  age: 20,
  message: "random message",
});

const changeMessage = () => {
  setPerson({ ...person, message: "hello world" });
};

// useState with a function
// Uses the current value instead of the value during calling the function
const complexIncrease = () => {
  setTimeout(() => {
    setValue((prevState) => {
      return prevState + 1;
    });
  }, 2000);
};
```

### useEffect

```javascript

```
