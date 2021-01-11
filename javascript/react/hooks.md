## Hooks

### useState

- The hook method has to be imported from react-package
- useState returns an array, where first element is the value and the second one is the handler
- Components which use hooks, have to start with upper case
- Must be in the function/component body
- Remember to use an arrow function e.g. in button for events, because otherwise the function will be invoked as soon as component renders, not on click or whatever event is used

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
const [person, setPerson] = useState({
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

- Runs after every re-render by default
- Supports second parameter (optional), an array, which makes useEffect run only if the elements in the array have changed
- An empty array means that useEffect will only run on the initial render

```javascript
// Simple example
useEffect(() => {
  console.log(`foo ${value}`);
}, [value]);

// Cleanup function for event listeners etc.
useEffect(() => {
  window.addEventListerner("resize", checkSize);
  return () => {
    window.removeEventListener("resize", checkSize);
  };
});
```

### useRef

- Preserves value
- Does not trigger re-render
- Popular use case is to target DOM nodes/elements

```javascript
const UseRefBasics = () => {
  const refContainer = useRef(null);

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(refContainer.current.value);
  };

  return (
    <>
      <form className="form" onSubmit={handleSubmit}>
        <div>
          <input type="text" ref={refContainer} />
          <button type="submit">Submit</button>
        </div>
      </form>
    </>
  );
};
```

### useReducer

- Similar to Redux, but not a direct replacement: Redux manages global state, useReducer component level state

```javascript
// Reducer function
// Usually reducer is moved into a separate file
const reducer = (state, action) => {
  if (action.type === "ADD_ITEM") {
    const newItems = [...state.people, action.payload];
    return {
      ...state,
      people: newPeople,
      isModalOpen: true,
      modalContent: "An item added",
    };
  }

  if (action.type === "NO_VALUE") {
    return {
      ...state,
      isModalOpen: true,
      modalContent: "Please enter an value",
    };
  }

  if (action.type === "CLOSE_MODAL") {
    return { ...state, isModalOpen: false };
  }

  if(action.type === "REMOVE_ITEM") {
    const newPeople = state.people.filter((person) => person.id !== action.payload);
    return {...state: people: newPeople}
  }

  throw new Error("No matching action type");
};

const defaultState = {
  people: [],
  isModalOpen: false,
  modalContent: "",
};

const Index = () => {
  const [name, setName] = useState("");
  const [state, dispatch] = useReducer(reducer);

  const handleSubmit = (e) => {
    e.preventDefault();
    if (name) {
      const newItem = { id: new Date().getTime().toString(), name };
      dispatch({ type: "ADD_ITEM", payload: newItem });
      setName("");
    } else {
      dispatch({ type: "NO_VALUE" });
    }
  };

  const closeModal = () => {
    dispatch({ type: "CLOSE_MODAL" });
  };

  return (
    <>
      {state.isModalOpen && (
        <Modal closeModal={closeModal} modalContent={state.modalContent} />
      )}
      <form onSubmit={handleSubmit} className="form">
        <div>
          <input
            type="text"
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
        </div>
        <button type="submit">Add</button>
      </form>
      {state.people.map((person) => {
        return (
          <div key={person.id}>
            <h4>{person.name}</h4>
            <button
              onClick={() => {
                dispatch((type: "REMOVE_ITEM"), (payload: person.id));
              }}
            >
              Remove item
            </button>
          </div>
        );
      })}
    </>
  );
};
```

### Custom hooks

```javascript
import { useState, useEffect } from 'react';

// Returns an object, which can be de-structured and used in the parent component
export const useFetch = (url) => {
  const [loading, setLoading] = useState(true);
  const [products, setProducts] = useState([]);

  const getProducts = async () => {
    const response = await fetch(url);
    const products = await response.json();
    setProducts(products);
    setLoading(false);
  };

  useEffect(() => {
    getProducts();
  }. [url]);

  return { loading, products };
}
```
