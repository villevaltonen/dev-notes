## Component

- Returns JSX always
- Returns only a single element (JSX-rule)
- In JSX:
  - use className instead of class
  - closing tags are required always
- Wrapping JSX into parentheses (); helps you keep the code more readable

Creating a components

```javascript
// Stateless functional component
// With regular syntax
function Greeting() {
  return <h4>This is my first component</h4>;
}

// With an arrow function
const Greeting = () => {
  return <h4>This is my first component</h4>;
};
```

Injecting React into index.html

```javascript
ReactDom.render(<Greeting />, document.getElementById("root"));
```

### Props

```javascript
function BookList() {
  return (
    <section>
      <Book title="foo" author="john doe" />
      <Book title="bar" author="jessie doe" />
    </section>
  );
}

const Book = (props) => {
  // This is also an option for accessing props
  // const { title, author } = props;
  // or
  // const Book = { title, author } => { ...
  return (
    <article className="book">
      <h1>{props.title}</h1>
      <h4>{props.author}</h4>
    </article>
  );
};
```

### Children

- Component can have children props between opening and closing tags, when you are creating a component

Accessing children props

```javascript
<p>{props.children}</p>
```

### Rendering a list of objects

```javascript
const books = [
  {
    id: 1,
    title: "foo",
    author: "bar",
  },
  {
    id: 2,
    title: "bar",
    author: "foo",
  },
];

function BookList() {
  return (
    <section>
      {books.map((book) => {
        return <Book key={book.id} book={book}></Book>;
        // With spread operator
        // If used, props.book is not needed in Book-component, replace it with "props" or destructure properties in parameters with { title, author } syntax
        return <Book key={book.id} {...book}></Book>;
      })}
    </section>
  );

const Book = (props) => {
  const { title, author } = props.book;
  return (
    <article className="book">
      <h1>{title}</h1>
      <h4>{author}</h4>
    </article>
  );
};
```
