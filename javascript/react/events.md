## Events

```javascript
const Book = ({ img, title, author }) => {
  // Separate function, inline function example below
  const clickHandler = () => {
    alert("hello world");
  };
  return (
    <article className="book">
      <h1 onClick={() => console.log(title)}>{title}</h1>
      <h4>{author}</h4>
      <img src={img} alt="" />
      <button type="button" onClick={clickHandle}>
        Example
      </button>
    </article>
  );
};
```
