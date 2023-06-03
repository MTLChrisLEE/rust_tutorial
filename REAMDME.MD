#Hello World

 * Run "cargo new hello"
 * Run "cago build"
 * Run "cargo run"  


#Macro

 * Ends with !
 * Basically a segment of cod that is given to name



#Data types


## Numbers 
Integer:
 - 8 bit 
 - 16 bits 
 - 32 bits 
 - 64 bits  
 - 128 bits 

Floating
 - f32
 - f64

##Characters and Boolean
Boolean
 - true
 - false

Character
 - 'a'


```
let x: i8 = 10
let y: u8 = 10
 
```


##Tuple
 * A general way of grouping together a number of values with a variety of types into 1 compoundt type
 * Fixed length
 * Use paranthesis
 * Accessing elements with . operator
 * The other way to access elements is to destructure the tuple


```
let tup = (500, "hi", true)
println!("{}", tup.0)
println!("{}", tup.1)
println!("{}", tup.2)

let (x,y,z) = tup; //Destructuring a tuple, the better way

println!("{}", x)

```

##Array
 * Every element of an array must have the same type
 * Fixed size
 * Use squre brackets
 * Accessing eleents with [] operator

```
let arr = [0,1,2,3,4]

println!("{}", array[0])

let arr2: [i32; 3] = [4,5,6];
arr2[0] = 10; //Error, because it's not mutable

```


##String and String slices

  * String
    * Vector of u8 data
    * Mutable
    * Stored on the heap (Because it can grow and shrink in size. The size is not constant, so cannot be on the stack)

  * String slice (&str)
    * Vector of u8 data
    * Immutable
    * Can be stored on the heap, stack or embedded in the compiled code

  
  ``` Casting &str to String
  fn main() {
      let person_name_slice; "Chris LEE";
      let person_name_string = person_name_slice.to_string();
      //or
      //let person_name_string = String::from("Chris LEE")

  }

  ```


  ```Casting String to &str
       let person_slice = &person_name_string;
       //or
       //let person_slice = person_name_string.as_str();

  ```



#Variables

##Casting

 * In Rust, there is no implicit casting
 * Rust is picky


##Immutable

 * By default, in Rust, variables are immutable
 * To make it mutable, use "mut" keyboard

 let mut x = 0;



#Control Flow


## if/else if and else


```
let word = "Dog"
if word == "Duck" {

}else if word == "Dog" {

} else {

}
```

## Enum


```
enum NavigationAids {
        NDB,
        VOR,
        VORDME
        FIX {name: String, latitude: f32, longitude:f32}
    }


fn main() {
        //By default, enum values are int
        //But we need to cast it
        //Because Not all values have type, 
        println!("NDB:\tP{}", NavigationAids::NDB as u8)
    }



```




##Option


Rust does not have a 'null' data type.
 * Why? 
   * It is dangerous
   * Cause memory leaks

In Rust, we use Option
  * Has 2 values
    * Some (value exists)
    * None (no value exists)



```
fn main() {
    let phrase = String::from("Daeng Daeng")
    let letter = phrase.chars().nth(3)
    //Type of letter variable is Option<char>


    //match statement is used to evaluate Option<?>
    let value = match letter {
        Some(character) => character.to_string(),
        None = String::from("No Value")
    }
}

```


## match operator

  * Similar to 'switch' statement in other languages



```
fn main() {
    let animal = "Duck"
    match animal {
        "Duck" => println!("Quack")
        "Dog" => println!("Woof")
        _ => println!("All quiet out here")
    }

}


```



## if let




```
fn main() {
    let animal = "Duck"


    //It's not a mistake, one = sign is good.
    //Refutability of "let"" keyword
    if let animal = "Duck" {
        println!("QUACK")
            
    }

``

  * Refutability test
    * See if the value that we want to assign to the variable is or can be different than current value



##Loop


###loop 

  * Runs forever


```
fn main() {
    loop {
        println!("Hello")

    }
}
```

```
fn main() {
    let mut counter = 0;
    loop {
        counter += 1;
        println!("{}", counter);
        if counter == 10  {
            break;
        } 

    }
}
```

###while

```
fn main() {
    let mut counter = 0;
    while counter <= 10{
        counter += 1;
        println!("{}", counter);
    }
}
```

##For loop

```
fn main() {
    for index in 1..=10 {
        println!("{}", index)
        
    }
}    
```

Using Iterato
```
let aircracts = ["737", "767", "787"]


for aircraft in aircrafts.iter() {
    println!("{}", aircraft)
}



```

#Ownership and Borrowing

##Ownership

  * There can be 1 and only 1 owner of data at a time

```
fn main() {
    let original = String::from("Original Value")
    println!("\noriginal: \t\"{}\"", original)

    let next = original;
    println!("{}", original);

}
```
Compile error. borrow of moved value: 'original'


 1. We are declaring original which is a string. Strings can change size so their data is stored in the heap
 2. The variable original is a pointer which lives on te stack and points to the memory location where the string value of original value is
 3. When we declare next, we're setting it equal to original and by setting next equal to original, we are handing over the ownership of that memory from original to next. 
 4. original no longer exists


##Borrowing

  * Allows another variable to take temporary ownership of data without deallocating the original variable


```
fn main() {
    let original = String::from("Original Value")
    println!("\noriginal: \t\"{}\"", original)

    let next = &original;
    println!("{}", original);

}
```
Nothing is mutable.
next is a read-only pointer to the memory
original still owns the memory but "next"" can read the value at the memory address that original is poiting to.




```
fn main() {
    let mut original = String::from("Original Value")
    println!("\noriginal: \t\"{}\"", original)

    let next = &original;
    println!("{}", original);

}
```
Nothing changes
The data in the memory location that original is pointing to can change but it's not changing




```
fn main() {
    let mut original = String::from("Original Value")
    println!("\noriginal: \t\"{}\"", original)
    original = String::from("New value");

    let next = &original;
    println!("{}", original);

}
```
Still ok because noting is changing after we're borrowing original



```
fn main() {
    let mut original = String::from("Original Value")
    println!("\noriginal: \t\"{}\"", original)

    let next = &original;
    original = String::from("New value");
    println!("{}", original);

}
```
We have a problem.
While there are read-only references to a variable that are being used downstream, you cannot change the value 


```
fn main() {
    let mut original = String:;from("Original value")
    println!("\nOuter original value: \t\"{}\"", original)


    {
        let next = &mut original
        *next = String::from("next value")

        println!("Inner scope next: \t\"{}\"", next)
        println!("Inner scope original: \t\"{}\"", original)

    }

    println!("Outer scope original: \t\"{}\"", original)
}

```
Original value
next value
next value
next value


##Lifetime

Lifetimes only apply to data that is stored on the heap


Dangling ref
```
fn main() {
    let outer_scope;
    {
        let inner_scope = 5;
        outer_scope = &inner_scope;
    }
    println!("{}", outer_scope);
}
```



```
fn main() {
    let returned_ref = return_bad_ref()

}


fn return_bad_ref() -> &i32 {
    let value = 5;
    &value

}

```





#Vectors
 * Resizable
 * Allocated on the heap


```
let mut nums = vec![1,2,3];

nums.push(4)
println!("{:?}", nums);
nums.pop()
println!("{:?}", nums);


let mut vec = Vec::new();
vec.push("Test")
vec.push("String")




```
