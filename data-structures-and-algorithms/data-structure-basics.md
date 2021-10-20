## Basics

### Algorithm basics

- Algorithm is a peripheral sequence of actions
- Executable in a peripheral period of time
- Executable with a peripheral amount of work
- Deterministic

### Complexity and performance

- Algorithm must return the correct result
- Easy to understand and implement
- Efficient use of time and memory
- Performance

- Time complexity
- Space complexity
- Hardware complexity

### Classification of time complexity functions

- Exponential time complexity
  - E.g. 2^n, 3^n etc.
  - Useful only for small inputs
  - In practice: 2*2, 2*2\*2 etc.
- Polynomial time complexity
  - E.g. Square root of n, n, n², n³, nlogn etc.
  - Most common
  - Efficient...somewhat efficient
  - n is linear
  - n² is quadratic
- Logarithmic time complexity
  - Binary logarithm (i.e. how many times a number can be halved)
  - E.g. logn, loglogn etc.
  - logn is almost always log2n in computer science
  - All functions, which are less than O(n) are below linear time complexity
    - All elements can't be handled or are not needed to handle
  - E.g. binary search
  - logn is inverse of 2^n, i.e. we calculate how many number 2 exists, for example, in 2*2*2
    - Logarithm of 1000 is 10
    - Logarithm of 1 000 000 is 20
    - Logarithm of 1 000 000 000 is 30
  - Logarithm can be calculated
    - How many numbers are in the binary format of the number, e.g. 1000 is 1000 0000 00 => 10

### Order of maginitude rules

- Any logarithmic grows slower than polynomial
- Exponential grow faster than polynomial
- n grows slower than nlogn

### Sum rule

- Interpretation: time complexity of a sequence of executable actions is determined by the slowest
- Less meaningful terms can be left out due sum rule

### Product rule

- Interpretation: positive constant multipliers can be left out

### Instructions for calculating execution time

- Calculate from inside out
- Assignment, read and print actions are usually O(1)
  - Big structures are an exception
  - Type of the parameter affects
    - An array as value parameter O(n)
    - An array as a variable/reference parameter O(1)
- Giving a value to an expression O(1)
  - Except calling a subroutine
- Reference to an array O(1)
- Sequence of actions
  - With sum rule
- Conditional actions
  - Usually the worst case
- Take into account the time complexity of operations like contains(), remove() etc. depending on the given data structure

### Comparison of growth speed

- Can be solved like inequality

### Invariants

- Invariant is something that does not change

### Measuring the time complexity

- If the input is doubled and the time
  - doubles => linear
  - becomes slightly bigger than doubled => e.g. O(nlogn)
  - quadruples => quadratic
  - eightfolds => cubic
- If input is increased by one and the time spent doubles => exponential (2^n)
