## Quick sort

- In each phase, the elements of the array are partitioned into two partitions based on some condition (this node is called pivot)
- After partitioning, the pivot node is left in place and rest are sorted recursively
- Divide and conquer

```java
// The whole array is sorted by calling quicksort(A, 0, A.length-1)
public static <E extends Comparable<? super E>> void quicksort(E[]A, int i, int j) {
  if(i < j) {
    int pivot = partition(A, i, j); // jaottelu
    quicksort(A, i, pivot-1); // alkuosa
    quicksort(A, pivot+1, j); // loppuosa
  }
}
```

### Partitioning algorithm

- in the beginning of the subroutine "partition", pivot is stored, so we will have an empty spot to the array
- if and when pivot is chosen from somewhere else than beginning, the first element of the array is moved to the old position of the pivot
- inspect the partition in turns from different ends until partition is done
  - when an element, which is smaller than the pivot, is found from the second partition, it is moved to the empty spot in the beginning of the array => an empty spot is left into the end of the array
  - when an element, which is larger than the pivot, is found from the first partition, it is moved to the empty position in the end => an empty spot is left into the beginning of the array
  - finally the pivot is placed in the empty position, which is left in the end of the phase
    - all smaller elements are on the other side and all bigger ones are on the other
    - the partition is now split into smaller and bigger sections and these can be sorted recursively without any dependencies between them
    - partition algorithm returns the final position of the pivot to the quick sort
- there are other variations as well, but the principles and the result are the same

### Choosing the pivot

- pivot does not effect to the accuracy of the algorithm, but it is effecting the time complexity
- The best time complexity is achieved with equal sized partitions
- The easiest way is to pick a random element as pivot (could be random, the one in the middle, the first etc.)
- Because pivot is very critical for time complexity, it is okay to use some time while choosing a pivot
- Pivot is chosen once on each level of recursion, partition() takes O(n)
- Time complexity
  - If pivot was always in the middle, the whole algorithm would be O(nlogn)
  - If pivot was always chosen poorly, time complexity would be O(n²)
  - Analysing the average time complexity is hard so we determine it to be O(nlogn)
  - Space complexity depends on recursion (amount of levels)
    - O(logn) at best and on average
    - O(n) at worst
- kth smallest or largest element
  - sorting, index k: O(nlogn) or O(kn)
  - priority queue: O(nlogk)
- quick sort can be utilized for searching, when only one of each recursions (partition) is called based on condition (smaller or larger than k (pivot))
