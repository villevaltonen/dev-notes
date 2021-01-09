## Forms

- onClick can be used on button
- onSubmit can be used on form

```javascript
// For a small amount of inputs
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
            <label htmlfor="firstName">Name: </label>
            <input
              type="text"
              id="firstName"
              name="firstName"
              value={firstName}
              onChange={(e) => setFirstName(e.target.value)}
            />
          </div>
          <div className="form-control">
            <label htmlfor="email">Name: </label>
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

// For larger forms
const ControlledInputs = () => {
  const [people, setPeople] = useState([]);
  const [person, setPerson] = useState({
    firstName: "",
    email: "",
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    if (person.firstName && person.email) {
      const newPerson = { ...person, id: new Date().getTime().toString() };
    }
    setPeople(...people, newPerson);
  };

  const handleChange = (e) => {
    const name = e.target.name;
    const value = e.target.value;
    setPerson({ ...person, [name]: value });
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
              value={person.firstName}
              onChange={handleChange}
            />
          </div>
          <div className="form-control">
            <label htlmFor="email">Name: </label>
            <input
              type="text"
              id="email"
              name="email"
              value={person.email}
              onChange={handleChange}
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
