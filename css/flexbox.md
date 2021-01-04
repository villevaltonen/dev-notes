## Flexbox
- a display type with range of properties
- an alternative to floats and display types
- has two main parts
    - container
        - parent element with display type active
        - usually div
    - items
        - child elements
        - make up the contents of the box

### creating a flex box
```css
display: flex;
```

### flex-direction
```css
flex-direction: row;
flex-direction: row-reverse;
flex-direction: column;
flex-direction: column-reverse;
```

### flex-wrap
```css
flex-wrap: wrap;
flex-wrap: nowrap;
```

### justify-content
```css
justify-content: flex-start;
justify-content: flex-end;
justify-content: center;
justify-content: space-between;
justify-content: space-around;
```

### align-items
```css
align-items: center;
align-items: flex-start;
align-items: flex-end;
align-items: baseline;
align-items: stretch;
```

### flex-grow
- needs to be added to every element
- default value is 0
```css
flex-grow: 1;
```

### flex-shrink
- defines how much elements shrink in relation to others
- default value is 1
```css
flex-shrink: ;
```

### flex-basis
- minimum width for an element
```css

flex-basis: 100px;
```
### flex
- basically combines the three previous properties (flex-grow, flex-shrink, flex-basis)
```css
flex: 5 1 100px;
```
    
### align-self
- allows aligning items vertically individually
```css
align-self: flex-start;
align-self: flex-end;
align-self: center;
```