## Grid
- works on both directions (width, height)

### Creating grid
- added on a container
```css
display: grid;
/* grid-template-columns: 10px 50px 10px; */
grid-template-columns: auto auto auto;
/* grid-template-rows: 50px 250px; */
grid-template-rows: auto auto;
```

### Aligning content
- added on a container
```css
/* justify-content */
justify-content: start;
justify-content: end;
justify-content: center;
justify-content: space-evenly;
justify-content: space-around;

/* align-content */
align-content: space-between;
align-content: space-evenly;
align-content: start;
align-content: end;
```

### Gaps
- added on a container
```css
grid-column-gap: 150px;
grid-row-gap: 300px;
/* combines both */
grid-gap: 300px 150px;
```

### Column and row lines
- added on an element
```css
/* make an element two columns wide */
/* grid-column: 1 / 3;  */
grid-column: 1 / span 3;
/* make an element two rows high */
/* grid-row: 1 / 3;  */
grid-row: ; 2 / span 2;
```

### Area
- basically a short hand for column and row lines
- added on an element
```css
/* row start, column start, row end, column en */
grid-area: 2 / 1 / span 2 / span 3;
```