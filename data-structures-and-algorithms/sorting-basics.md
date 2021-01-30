### Sorting basics

- Sorting means putting the elements of a collection to a linear order
- Internal sorting
  - Means all elements can be fitted into memory
  - Easiest to do in an array
    - Direct access to index
    - Often sorting is done by changing elements between each other
    - Or a supporting structure is used as an temporary location for some or all elements (usually means linear requirement for addition space)
  - Sorting a list is fast too
    - Handling only sequential elements is efficient (predecessor/successor)
    - Adding into the middle and splitting the list is easy as long as we have a reference to a list node
    - Additional space is not usually needed
- External sorting
  - Means memory is not enough and persistent storage is needed
- Sorting key
  - Ascending order is determined with sorting key
  - Sometimes multiple keys are needed
  - Completely perfect order is not necessarily needed
