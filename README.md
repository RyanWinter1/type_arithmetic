Naive implementation of Peano arithmetic solely in rust's type system. Peano arithmetic is essentially a simple but powerful way of defining the natural numbers (0, 1, 2, 3 etc), where we define natural numbers inductively as either Zero or Successor of a natural number. 
For more information: [Peano Axioms](https://en.wikipedia.org/wiki/Peano_axioms)

For example: 


```
type One = Succ<Zero>;
type Two = Succ<One>;
type Three = Succ<Two>; 

<One as Add<Two>> == Three; //These types are equal, but this line of code is not valid rust; see next line for how to directly compare the types.

assert_types_eq!(<One as Add<Two>>::Result, Three); //Checking the types are equal using simple homemade macro

<One as Mul<Two>> == Two //We can also use multiplication!



```

There is also a macro to easily create the desired integer types:

```
type TwentyFive = int_to_type!(25);
type OneHundred = int_to_type!(100); //expands to Succ<Succ...<Zero>>>... with 100 Succs, which represents the number 100.

assert_types_eq!(OneHundred, <TwentyFive as Mul<Four>>::Result);
```

Q: Why is this useful? Aren't we just making the compiler do slow math while compiling?

A: Yes, this implemention is incredibly slow for any calculation that isn't with tiny integers, and was written to play around with Rust's type system. For an efficient implementation, you can look at the [typenum](https://github.com/paholg/typenum) crate. The crate that first got me interested in this was the [dimensioned](https://github.com/paholg/dimensioned) crate, which uses type level arithmetic to give you dimensional safety and tell you off at compile time when you try and add 5 metres to 20 seconds. Another cool crate is the [generic-array](https://github.com/fizyk20/generic-array) crate, which uses type-level arithmetic to create generic-length arrays.


Q: Is this not incredibly inefficient?

A: Yep. On my laptop, any number greater than about 1600 can't be represented; Try it yourself, int_to_type!(1500) will probably compile but int_to_type!(2000) probably won't.