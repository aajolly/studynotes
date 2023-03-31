## Primitive Data Types
### Boolean
- Values are true or false
- Can't convert to other data types
- Zero value = false

### Numeric
#### Integers
- Signed integers
  - int type has varying size, but min 32bits
  - 8 bit(int8) through 64 bit(int64)
- Unsigned integers
  - 8 bit (byte and uint8) through 32 bit (uint32)
  - Only store positive numbers
- Arithmetic operations
  - Addition, subtraction, multiplication, division, remainder
- Bitwise operations
  - And, or, xor, and not
- Zero value = 0
- Can't mix types in same family! (uint16 + uint32 = error)

#### Floating point numbers
- Follow IEEE-754 standard
- Zero value = 0
- 32 and 64 bit versions
- Literal styles
  - Decimal (3.14)
  - Exponential (13e18 or 2E10)
  - Mixed (13.7e12)
- Arithmetic operations
  - Addition, subtraction, multiplication, division

### Text
#### Strings
- UTF-8
- Immutable
- Can be concatenated with (+) operator
- Can be converted to []byte
#### Rune
- UTF-32
- Alias for int32
- Special methods normally required to process
  - e.g. strings.Reader#ReadRune

## Constants
- Immutable, but can be shadowed
- Replaced by the compiler at compile time
  - Value must be calculable at compile time
- Named like variables
  - PascalCase for exported constants
  - camelCase for internal constants
- Typed constants work like immutable variables
  - Can interoperate only with same type
- Untyped constants work like literals
  - Can interoperate with similar types

#### Enumerated contsants
- Special symbol `iota` allows related constants to be created easily
- `Iota` starts at 0 in each const block and increments by one
- Watch out of constant values that match zero values for variables

#### Enumerated expressions
- Operations that can be determined at compile time are allowed
  - Arithmetic
  - Bitwise operations
  - Bitshifting

## Arrays and Slices
### Arrays
- Collection of items with same type
- Fixed size
- Declaration styles
  - a := [3]int{1, 2, 3}
  - a := [...]int{1, 2, 3}
  - var a [3]int
- Access via zero-based index
  - a := [3]int {1, 2, 3} // a[1] = 2
- `len` function returns size of the array
- Copy refers to different underlying array

#### Slices
- Backed by array
- Creation styles
  - Slice existing array or slice
  - Literal style
  - Via make function
    - a := make([]int, 10) // create slice with capacity and length == 10
    - a: = make([]int, 10, 100) // slice with length == 10 and capacity == 100
  - `len` function returns length of slice
  - `cap` function returns length of the underlying array
  - `append` function to add elements of slice
    - May cause expensive copy operation if underlying array is too small
  - Copy refers to same underlying array

## Maps and structs
### Maps
- Collections of value types that are accessed via keys
- Created via literals or via make function
- Members accessed via [key] syntax
  - myMap["key"] = "value"
- Check for presence with "value, ok" form of result (think of Python's get() method for dictionary)
- Multiple assignments refer to same underlying data

### Structs
- Collections of disparate data types that describe a single concept
- Keyed by named fields
- Normally created as types, but anonymous structs are allowed
- Structs are value types
- No inheritance, but can use composition via embedding
- Tags can be added to struct fields to describe field
