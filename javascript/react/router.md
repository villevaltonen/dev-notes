## Router

- External package: `react-router-dom`

```javascript
// App
import { BrowserRouter as Router, Route, Switch } from "react-router-dom";

const App = () => {
  return (
    <Router>
      <Navbar />
      <Switch>
        <Route exact path="/">
          <Home />
        </Route>
        <Route exact path="/about">
          <About />
        </Route>
        <Route exact path="/people">
          <People />
        </Route>
        <Route path="/:id" children={<Person />}></Route>
        <Route exact path="*">
          <Error />
        </Route>
      </Switch>
    </Router>
  );
};

// Navbar
const Navbar = () => {
  return (
    <nav>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
        <li>
          <Link to="/people">People</Link>
        </li>
      </ul>
    </nav>
  );
};

// Error
const Error = () => {
  return (
    <div>
      <h1>Error Page</h1>
      <Link to="/" className="btn">
        Back Home
      </Link>
    </div>
  );
};

// People
const People = () => {
  const [people, setPeople] = useState([]);
  return (
    <div>
      <h1>People Page</h1>
      {people.map((person) => {
        return (
          <div key={person.id} className="item">
            <h4>{person.name}</h4>
            <Link to={`/person/${person.id}`}>Learn more</Link>
          </div>
        );
      })}
    </div>
  );
};

// Person
import { Link, useParams } from "react-router-dom";

const Person = () => {
  const [name, setName] = useState("default name");
  const { id } = useParams();

  useEffect(() => {
    const newPerson = data.find((person) => person.id === parseInt(id));
    setName(newPerson.name);
  }, []);

  return (
    <div>
      <h1>{name}</h1>
      <Link to="/people" className="btn">
        Back to People
      </Link>
    </div>
  );
};
```
