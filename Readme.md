# The Rust Programming Language Take Note
## Constants
    Not allow to change.
    Aren't  allow to use mut with contants.
    Always immutable.
    Declare constants using : const keyword
    Contants may be set to only  a contant expression, not a result of a value that could only be computed at runtime.
## Shadowning
    fn main() {
        let x = 5; // shadowed

        let x = x + 1; //overshadows

        {
            let x = x * 2;
            println!("The value of x in the inner scope is: {x}");
        }
    println!("The value of x is: {x}");
    }

    Shadowning is difference between mu:
        1. We can perform a few tranformations on a value  but have the variable be immutable after those tranformations have been completed.
        2. We can change the type of a value but reuse the same name.
## Data types
    Two main data types subset:
        1. Scalar
        2. Compound
Rust is statically typed language -> must know the types of all variables at compile time.

### Scalar types
    Represent a single value.

    Four primary scalar type:
        1. Integers
        2. Float-point
        3. Numbers
        4. Booleans
        5. Characters
#### Integer Types
    Is a number without a fractional component
    Each signed variant can store numbers from -(2n - 1) to 2n - 1 - 1 inclusive,
    Integer literals form: Decimal : 1234, Hex: 0xfff, Binary: 0b11100, Byte( u8 only): b'A'
    Default type of integer will be use: i32
**Integer Overflow**: 
1. When compiling in debug mode -> will procedure panic error.
2. When  compiling in release mode -> the program won't panic. perform two's complement wrapping, Example in case u8, the value 256 will becomes 0, 257 become 1.
#### IntegeFloat-point Types

#### Numeric Operations
    Rust supports the basic mathematical operations:  addition, subtraction, multiplication, division, remainder.
    Integer division toward zero to the neast integer.
#### Boolean Type
    Boolean are one byte in size.
#### Character Type
    char literals using single quotes as oppose to string literals using double quotes
    Rust charater is 4 byte in size represent unicode scalar value.
### Compound Types
    Can group multiple value into one type.
    Rust have two primitive compound types:
        1. Tuples
        2. Arrays

#### Tuples Types
    Group multiple together a number of values with a variety of type into one compound type.
    Tuples have a fixed length.
    The tuple without any value has special name: unit. It represent an empty value or empty return type.

#### Arrays Types
    Arrays in Rust have a fixed length.
    Arrays allocated data on stack.
    Using when know the number of elements will not need to change.
    Invalid array elements access: result error at runtime. Difference from other lowlevel languege, will be allow to perform access invalid index on the arrays.
## Functions
    Rust is expression-based language.
    **Statement** are instructions that perform some action and **do not return a value**.
        Creating and assigning a value.
        Function defination
        In C: Statement return the value of the assignment.

    **Expression** evaluate to a value.
        Do not include ending semicolons.
        Calling function.
        Calling macro.

        A new scope created with culy is  an expression.
        fn main() {
            let y = {
                let x = 3;
                x + 1
            };

            println!("The value of y is: {y}");
        }
### Return value
    The return value is synonymous with the value of the final expression in the block of the body of a funciton.
    Can return early by using return keyword and specify a value.
    
    fn five() -> i32 {
        5
    }

}
