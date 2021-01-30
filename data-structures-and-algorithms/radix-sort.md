## Radix sort

- Sorting is done in phases by the parts of keys
- Briefly:
  1. Keys are divided into reasonable sized partitions
  2. Sorted based on the least significant part of the key
  3. Continue...
  4. Finally based on the most significant part of the key
- Cutting the keys is recommended to be done by remainder arithmetic or bit operation based on the type
- Sorting phases have to be stable, which means that equal elements (based on the parts) have to remain in the original order
