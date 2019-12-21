# Data Structures

Built-in data structures that are part of the Nox standard library, and have
first-class language syntax support are:


* Atoms (aka Scalars, aka Primitives)
  * Int (e.g. `1`, `2`, `9`)
  * Float (e.g. `1.3333333`, `3.141529`)
  * String (e.g. `'hai'`, `"hello"`, `` `hola` ``)


* Algebraic Data Types
  * Bool (e.g. `True` | `False`)
  * Maybe type (e.g. `T?`, `undef`)
  * Result type (e.g. `Ok`, `Err`)


* Non-Scalars
  * `Date` / `DateTime` - tempted to avoid these types because they are so ambiguous.  Instead, can we break this down into either:
    * `Timestamp` (e.g. a moment in time, represented by a single Float, a Unix timestamp)
    * `Moment` (e.g. represented by a Timestamp + a timezone)
    * `Date` (e.g. calendar date, represented by three ints, YYYY-MM-DD, not an exact moment in time)
    * `Time` (e.g. time of day, represented by two ints and a float, 23:59:59.999, not an exact moment in time)


* Collections
  * List (e.g. `[]`, `[x]`, `[x, ..xs]`) - are backed by arrays, not linked-lists
  * Set (e.g. `Set.empty`, `{1}`, `{1..5}`)
  * Dict (e.g. `Dict.empty`, `{ a: 1, b: 2 }`)
  * Tuple (e.g. `(1, 2)`, `(x, y)`)


# Atoms

The built-in primitive data types in Nox are called Atoms.

- Int
- Float
- String


Besides the Atoms, Nox defines another class of basic types, which are the
Algebraic Data Types (ADTs).

- Boolean
- Maybe<T>


Built-in collection types are:

- List
- Record
- Dict
- Set


## Int

Integers are whole numbers, in the base of 10.

Literals:

    0, 1, 2, 3, ...     // Positive numbers
    0, -1, -2, -3, ...  // Negative numbers
    -0                  // Same as 0

Thousands separators are allowed for readability.  These `_`'s carry no
meaning and are only allowed in the syntax to increase readability:

    100_000    // 100K
    1_000_000  // one million

They must be used as thousands separators, not for other purposes.  For
example, the following inputs will be parse errors:

    12_34_56
    1_2_3_4_5_6


## Float

Floats are natural numbers with a decimal point and an arbitrary precision that
is defined by the target low-level language Nox compiles to.

    0.0
    2.6666667
    -1.3333333

Float can use thousands separators, too:

    1_234.56
    100_000.0

The following isn't a valid float:

    .3                // Should be 0.3
    0.333_333_333     // Thousands separators can only be used on the left hand side of the floating point


## String

Strings are unicode strings.

    'Hello, Nox'  // Strings are single quoted
    "I'm Nox"     // Double quoted allowed if there's a single-quote char (') in the literal
    "She said: \"I love Nox\""  // Escaping happens with \"

Multi-line strings are simply strings that are allowed to have the `\n`
character in them:

    `
      The poet waits quietly
      to paint the unsaid

        –Atticus
    `

This example is the same as `'The poet waits quietly\nto paint the unsaid\n\n –Atticus'`.
Note how whitespaces are trimmed off the start and end of the string, as well
as all the indentation removed from the start of the "lines".  If this is
undesirable, and you want to use a multi-line string literal that retains the
whitespace exactly, use double backticks:

    ``
      The poet waits quietly
      to paint the unsaid

        –Atticus
    ``

In this case, the string literal will be the same as
`'\n  The poet waits quietly\n  to paint the unsaid\n\n  –Atticus\n'`.

Multi-line string literals are also template strings literals:

    occupation = 'poet'
    verbs = ['wait', 'paint']
    poet = { name = 'Atticus' }

    poem = `
      The {occupation} {verb[0]}s quietly
      to {verb[1]} the unsaid

        –{poet.name}
    `

