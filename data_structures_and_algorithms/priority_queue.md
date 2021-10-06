## Priority queue

- Like in list operations, the handling order of set operations can be limited/adjusted
- Priority is assigned to the elements
  - Higher priority elements is handled earlier
  - All elements with equal priority are handled before lower priority elements
  - Often (by default) smaller number means higher priority
  - If priority is the order of arrival = a queue
  - If priority is the inverted order of arrival = a deque
- Deciding the priority
  - To be able to calculate the order of removal, we have to be able to calculate priority for each element in the queue and priorities have to be comparable with each other
    - Options:
      - Priority is given as a number for each element when taken into the priority queue
      - Priority is calculated by a function (priority(x)), which is taking the type of the element into account
      - Priority is given as a priority function (compare(x1, x2))
      - The first example is the easiest, but less flexible
- Heap sort is based on priority queue
