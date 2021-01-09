## Prop-types

- A way to validate passed props
- `rafcp` for shortcut to create a functional component with proptypes
- `ptar` for shortcut `PropTypes.array.isRequired`

```javascript
import React from "react";
import PropTypes from "prop-types";
import defaultImage from "../../../assets/defaul-image.png";

const Product = ({ image, name, price }) => {
  return (
    <article className="product">
      <h4>single product</h4>
      <img src={image.url} alt={name} />
      <h4>{name}</h4>
      <p>{price}</p>
    </article>
  );
};

Product.propTypes = {
  image: PropTypes.object.isRequired,
  name: PropTypes.string.isRequired,
  price: PropTypes.number.isRequired,
};

//
// Product.defaultProps = {
//   name: "default name",
//   price: 3.99,
//   ...
};
```
