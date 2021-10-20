## Set

- Elements of a set are distinct
- Type of the elements is not limited
- Elements are the same type though
  - null is allowed/disallowed depending on the situation
- Often there is an expected linear order for the elements ("an ordered set")
- Interface: java.util.Set
- Default implementation, which is usually used: java.util.HashSet
  - Implementation with open hash:
    - Add, remove and contains are usually/on average O(1)
      - If runs out of space, restructuring takes linear time
    - Iterating only in random order, can be changed over multiple iterations
    - java.util.LinkedHashSet<E>
      - Like HashSet, but a linked list of elements is added
      - The list is used for iterating the set, so elements can be iterated in the adding order
      - Needs more resources than HashSet
    - java.util.NavigableSet (and SortedSet)
      - Elements must implement Comparable-interface (can be ordered)
      - Elements can't be changed so that the result of comparsion changes
        - Adding after change and removal are allowed
    - java.util.TreeSet
      - Based on balanced binary tree
      - Add, remove and contains are O(logn)