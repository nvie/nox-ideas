# Algebraic Data Types

Nox uses [Algebraic Data Types][wiki], or ADTs for short, as its base values.

A simple example is:

```nox
type Fruit = Apple | Orange | Mango
```

Here, `Fruit` is a so called "union type", or just "type", which can take
possible values of either `Apple`, `Orange`, or `Mango` -- called Atoms.  An
Atom is a unique "symbol".  Sometimes an Atom gets called a "Type Constructor"
(???).  Another way of thinking about this is that, in order to create
a variable of type `Fruit`, you'll have to assign it a _value_ of `Apple`,
`Orange`, or `Mango`.  These values aren't "strings" or "numbers" or "objects",
but instead more like "symbols".

Sometimes, you'll see the same definition spread across multiple lines, in
which case the "pipe symbol" gets prefixed before the first element, too:

```nox
type Foo =
  | Bar
  | Qux
```

When defining a type like this, sometimes you'll see that the type constructor
is using the same name as the union type itself:

```nox
type Bag = Bag
```

This may look confusing, but it's very useful, especially once you start using
type params.  For example:


```nox
type Bag<T> = Bag { items: T[] }
```

Here, there is a `Bag<T>` _type_, so you can have a variable of type
`mybag: Bag<T> = ...`.  In order to assign a value to that, you'd have to use
the `Bag` _type constructor_, for example, to create an empty bag, use
`Bag { items: [] }`.  This is the expression that would go on the `...` part.



## Standard library ADTs

Nox’s standard library ships with a bunch of default ADTs that are common and
very useful.  You may know these from other functional programming languages:

* [`Bool`](./Bool.md)` = True | False`
* [`Maybe`](./Maybe.md)`<T> = Just<T> | Nothing`
* [`Result`](./Result.md)`<E, T> = Ok<T> | Err<E>`


[wiki]: https://en.wikipedia.org/wiki/Algebraic_data_type
