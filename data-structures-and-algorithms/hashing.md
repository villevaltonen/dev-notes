## Hashing

- Storage space is an array or a file with m elements with indices 0..m-1
- Calculate an address for each element in the storage space
  - An integer between 0..m-1, which is can be calculated quickly
  - Hash function h, bigger than 0, smaller than m
  - Hash function must return the same value each time
- Sequential order is not usually preserved
  - Going through the elements can not be done sequentially directly, especially in memory
  - Going through in a random order can be done in time O(n+m)
  - Preparations for collisions are required, because in practice there are two or more elements ending up into the same place

### Closed hashing (open addressing)

- Storage space contains the actual elements
- If the home address is reserved, when adding an element, the element will be stored to the next closest available address
  - The next closest can be the next index (linear probing) or some other simple rule
  - If the element is not found from the home address, it can be in the other places based on the rule
    - Positions can be checked until an empty position is found
- Removing an element requires extra caution
  - Element is marked as deleted, so searches won't be interfered
- Good: a simple storage structure
- Bad: if the storage space is almost full, searches will take more time
- You should not allow the storage space to become close to full

### Open hashing (external hashing, chained hashing)

- Elements are always stored into their home address, but the home address is a list

### Double hashing

- Choose two or more completely different hashing functions
- If the first hash function collides, calculate the hash with the second hash function etc.
