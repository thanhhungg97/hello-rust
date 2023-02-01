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
## Control flow

### If expression
    Rust will not automically try to convert non-boolean  types to boolean.
    We must explict and alway provide if with boolean as its condition.
    Rust only executes the block  for the first true condtion
#### Using if in a let statement
    Because if is expression, we can use it on the right side of a let statement to assign the outcome to a variable.
    let number = if condition { 5 } else { 6 };
### Repetition with loop
    If we have loop within loop, break and continue will apply to the innermost loop at the point.
## Understanding Ownership
    Ownership help Rust to make memory safety gurantees without need any garbase collector.
    Ownership is the set of rules the the compiler check.
### Ownership rules
1. Each value in Rust has an owner.
2. There can only be owner at a time.
3. When the owner goes out of scope, the value will be dropped.

#### Variable scope
    {                      // s is not valid here, it’s not yet declared
        let s = "hello";   // s is valid from this point forward
        // do stuff with s
    }                      // this scope is now over, and s is no longer valid
   
        
    the memory is automically returned once the variable that owns it goes out of  scope.
    {
    let s = String::from("hello"); // s is valid from this point forward

        // do stuff with s
    }                                  // this scope is now over, and s is no
                                       // longer valid

    When a variable goes out of scope, Rust call a special function: drop.
#### Variables and Data Interacting with Move
    When assign a new reference to an old one, Rust will invalid the old reference.
    It known at a move.
    Rust will never create deep copies of your data by default.
#### Variables and Data Interacting with Clone
    If we want to do deep copy the heap data of String. not just the stack data.
    we can use the common method called clone.
    When call clone -> some code will executed slower.
####  Stack-only data: Copy
    The types which have adready know the size at compile time are stored entirely on the stack, so copy the actual data is quick.
####  Ownership and Function
    fn main() {
    let s = String::from("hello");  // s comes into scope
    
        takes_ownership(s);             // s's value moves into the function...
                                        // ... and so is no longer valid here
    
        let x = 5;                      // x comes into scope
    
        makes_copy(x);                  // x would move into the function,
                                        // but i32 is Copy, so it's okay to still
                                        // use x afterward
    
    } // Here, x goes out of scope, then s. But because s's value was moved, nothing
    // special happens.

#### Return value and scope
    Return value can also tranfer ownership.
    The ownership of a variable flows the pattern:
        assigning a value to another variable moves it.
        When a variables that include data on heap goes out of scope, the value will be clean up by drop unless the ownership of a data has been moved to another variable.
## References and Borrowing
    Refer to some value instead of taking ownership of it.
    The action of create reference -> borrowing.
    We are not allow to modify something we have a reference to.

### Mutable reference
    let mut s = String::from("hello");

    change(&mut s);

    if we have a mutable reference to a value, you can have no other reference to that value
    let mut s1 = String::from("hello");
    let tmp = &mut s1;
    let tmp1 = &mut s1; // error
    -> prevent multiple mutable reference to same data at the sametime.
    -> Prevent data race at compile time.
### Data race:
    Happend when:
        Two or more pointer access the same data  at the same time.
        At least one of the pointer is being used to write to the data.
        There's no mechanism being used to synchronize access to the data.
    Can use curly bracket to create new scope.
### Dangling References
    
    dangling pointer -> a pointer point to some memory that given to someone else by freeing some memory but preserving a pointer to that memory.
    fn dangle() -> &String { // dangle returns a reference to a String

    let s = String::from("hello"); // s is a new String

    &s // we return a reference to the String, s
    } // Here, s goes out of scope, and is dropped. Its memory goes away.
    // Danger!
## Slide type
    Let you reference a continous sequence of elements in a collection rather than the whole collection.
## Struct
    If the instance is multable, we can change the value by using dot notation and assigning to particular field.
    -> Entire instance must be mutable.
    Struct update syntax like an assignment -> this is because it move data.
    -> No longer using the instance as a whole.
    
#### Associated Functions
    All function define in imp block are associated functions
## Enums and Pattern Matching
    Each variant can have different types and amount of associated data.
### Match control flow structure
    matches Are Exhaustive

    _ is a special pattern that matches any value and does not bind to that value
    