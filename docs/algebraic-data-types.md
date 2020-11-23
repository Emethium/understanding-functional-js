## Algebraic Data Types (ADT's)

- Structured types that are formed by composing other types.
  
- Arrays, Objects, Maps, WeakMaps, and Sets are all algebraic data types.
  
- They are types that are able to mix **product** and **sum** types altogether.


### Product types

- Created by the combination of other types.

- Allows us to have more than on value on a single structure.

- Let's consider a trivial example in JavaScript using `superstruct` along with `crocks`:

  > Even if you know shit about those libraries (like me when first writing this doc), should be easy enough to read and understand.

  ```js
  const Pokemanz = object({
    name: string(),
    level: number(),
    types: array(string()),
    evolvesTo: array(string()),
  });
  ```

- This is called **product** type as all the possible entity types would be the product of all the possible values for `name` multiplied for all the possible values of `level` and so on.

- Feels a bit like an `AND` operation.


### Sum types

- Types where your value must be one of a fixed set of options.

- Kinda like `Enums`, but more flexible.

- Let's consider this another trivial example, following the last one's lead:

  ```js
  const Digimanz = object({
    name: string(),
    level: enums(['Rookie', 'Champion', 'Ultimate', 'Mega']), 
    attribute: enums(['Vaccine', 'Data', 'Virus', 'Free', 'Variable']),
    evolvesTo: array(string()),
  });

  // Actual values omitted for brevity
  const pokemanData = { ... };
  const digimanData = { ... };

  const fictionalEntity = either(pokemanData, digimanData);
  ```

  > Even though the name is really semantic, if don't know what the either function is doing here, please check [here](https://crocks.dev/docs/crocks/Maybe.html#either)

- Which would mean that our `fictionalEntity` can only be **one of the two** aforementioned types.

- Feels a bit like a `XOR` operation.

- Conceptually the `Either` type can hold a `Left` and `Right` value, but never both at the same time. We can put any data we want in said type, but only in one of those two slots.


### Final thoughts

- Learning functional programming is hard.

- At a basic level, ADT's are about mixing ANDs and OR's/XOR's.

- Algebraic Structures are a bit like code design patterns, but with a mathematical basis.

- Type classes are a way of implementing polymorphism.

- Algebraic data types are not the same as algebraic structures.
  - They are composite data types made of other types, classified as `Sum` and `Product` types.
  - Together, they’re like a dynamic duo for encoding business logic.



Sources:
 - [Functional Mumbo Jumbo - ADTs](http://blog.jenkster.com/2016/06/functional-mumbo-jumbo-adts.html)
 - [Algebraic Data types: Things I wish someone had explained about Functional Programming](https://jrsinclair.com/articles/2019/algebraic-data-types-what-i-wish-someone-had-explained-about-functional-programming/)