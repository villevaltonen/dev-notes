## Lists

### java.util.Vector/ArrayList

- A list implemented with an array
- Indexed positions
- get(i), set(i), add(i)/add(x,x) and adding or removing from the end of list are constant time
- add(i,x) to the beginning or in the middle are linear time complexity, because indices have to be updated

### java.util.LinkedList

- Time complexities differ from Vector/ArrayList
- Position is not exposed to the user
- Adding does not require moving/updating indices, so it is done in constant time
- Direct get(i) or add(i,x) are not good, because indices have to be calculated from the beginning of the list during the operation
- Adding to the beginning or to the end is O(n)
- Adding to a specific position of index is O(n²)
- Iterating O(n²) (compared to O(n) of ArrayList)
- Efficient iterating done with Iterator or ListIterator
  - Iterator: hasNext(), next(), remove() etc.

```java
ListIterator i = L.listIterator();
while(i.hasNext()) {
  ... = i.next();
}
```

- The collection cannot be altered while going through it
  - add- and remove-methods of the iterator are an exception
- If altering the collection is not needed during going through it, foreach can be used
