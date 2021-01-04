## Primitives

### Boolean type
- values are true or false
- not an alias for other types (e.g. int)
- zero value is false

### Numeric types
- integers
    - signed integers
        - int type has varying size, but min 32 bits
        - 8 bit (int8) through 64 bits (int64)
    - unsigned integers
        - 8 bit (byte and uint8) through 32 bit (uint32)
    - arithmetic operations
        - addition, subtraction, multiplication, division, remainder
    - bitwise operations
        - and, or, xor, and not
        - zero value is 0
        - can't mix types in same family (uint16 + uint32 = error)
    - floating types
        - follow IEEE-754 standard
        - zero value is 0
        - 32 and 64 bit versions
        - literal styles
            - decimal (3.14)
            - exponential (13e18 or 2E10)
            - mixed (13.7e12)
        - arithmetic operations
            - addition, subtraction, multiplication, division
    - complex numbers
        - zero value is (0+0i)
        - 64 and 128 bit versions
        - built-in functions
            - complex
            - real
            - imag
        - arithmetic operations
            - addition, subtraction, multiplication, division

### Text types
- strings
    - UTF-8
    - immutable
    - can be concatenated with + operator
    - can be converted to []yte
- rune
    - UTF-32
    - alias for int32
    - special methods normally required to process