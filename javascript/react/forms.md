## Forms

- onClick can be used on button
- onSubmit can be used on form

```javascript
const ControlledInputs = () => {
  const [firstName, setFirstName] = useState("");
  const [email, setEmail] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(firstName, email);
  };

  return (
    <>
      <article>
        <form className="form">
          <div className="form-control">
            <label htlmFor="firstName">Name: </label>
            <input
              type="text"
              id="firstName"
              name="firstName"
              value={firstName}
              onChange={(e) => setFirstName(e.target.value)}
            />
          </div>
          <div className="form-control">
            <label htlmFor="email">Name: </label>
            <input
              type="text"
              id="email"
              name="email"
              value={email}
              onChange={() => setEmail(e.target.value)}
            />
          </div>
          <button type="submit" onClick={handleSubmit}>
            Add person
          </button>
        </form>
      </article>
    </>
  );
};
```
