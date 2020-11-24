## Algebraic Structures

- In Math, and Algebraic Structure is on a set `A` is a collection of finitary operations on A.

- Collective term for well-known termos of the Functional universe like `monoids` and `functors` (we will figure all this guys out eventually).


### The Functor

- Functor is an algebraic structure that must have a `.map()` method with the following type signature (brace yourself):

  ```hs
  map :: Functor f => f a -> (a -> b) -> f b
  ```

  - You may be wondering what the hell is this shit. Well, in order to specify what types of parameters a functions takes and it's return, we have this beauty of a thing called **Hindley-Milner type signatures**. It can be read as:

    1. The `.map()` method takes a function as an argument.
    2. This function takes an argument of a generic type `a`, transforming this input into a generic type `b`.
    3. The transformed generic type `b` input is used as an argument for the original passing function, getting a Functor of `b`.
    4. **TL;DR**: The `.map()` method receives a function `f` that takes one argument, transforms this argument into something else and then applies this result into the former `f`.

- A Functor must obey two different laws:

  1. If `u` is a functor, calling `u.map(x => x)` must return `u`. This is called the **identity law**.
  2. If `u` is a functor and `f` and `g` are functions, then calling `u.map(x => f(g(x))` is equivalent to calling `u.map(g).map(f)`. This is called the **composition law**.
    > Which makes perfect mathematical sense, it always should be the same as resolving `g(x)` and then calling fe `f` function with the resolved value as the input.

- You may recall that, in JavaScript (and a shitload of other languages), the `Array` has a `.map()` method, classifying it as a Functor.

- Functor types are containers, but not all containers are functors.

- A functor does not care about the type it holds.

- For illustration purposes, without functors we would have all the primitive types as `Int`, `String`, `Number` ... but `Array` and `Set` wouldn't be able to exist at all.

- Following this line of thought, we can conclude that a Functor brings extended behavior to the table, with total type safety.

- **TL;DR**: Functor is a Algebraic Structure with one special method `map` that applies a function to the passing value and nothing else.


### What's the point?

- Algebraic Structures don't do anything on their own, they are just abstract definitions.

- Think back, why do we use classes? Because they abstract common behavior between a  bunch of objects. I turn, Algebraic Structures provide abstract common patterns between classes.

- They help us the same way all other abstractions helps us, hiding details so we can se the bigger picture.


Sources:
  - [Fantas, Eel, and Specification 6: Functor](http://www.tomharding.me/2017/03/27/fantas-eel-and-specification-6/)
  - [Algebraic Structures: Things I wish someone had explained about Functional Programming](https://jrsinclair.com/articles/2019/algebraic-structures-what-i-wish-someone-had-explained-about-functional-programming)