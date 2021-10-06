## Bin sort

- Keys or elements between 1..m, where m is reasonably sized (same order of magnitude as n at most)
- Briefly:
  1. m amount of bins are created
  2. Calculate the amount of elements, which will be placed into the different bins
  3. 1..m is only the key, which means the elements of bin array are lists of elements where the actual data is stored
  4. Start placing the elements into the final array bin by bin
- Time complexity O(m+n)
- It's worth noting, that elements are not compared to each other, only counted
- Bin sort is not good, if the values of keys (m) and therefore the amount of bins is big
  - E.g. floats or strings as keys
