# Data Structures

Built-in data structures that are part of the Nox standard library, and have
first-class language syntax support are:

* Scalars (or Primitives)
  * Bool (e.g. `True` | `False`)
  * String (e.g. `'hai'`, `"hello"`, `` `hola` ``)
  * Int (e.g. `1`, `2`, `9`)
  * Float (e.g. `1.3333333`, `3.141529`)

* Non-Scalars
  * `Date` / `DateTime` - tempted to avoid these types because they are so ambiguous.  Instead, can we break this down into either:
    * `Timestamp` (e.g. a moment in time, represented by a single Float, a Unix timestamp)
    * `Moment` (e.g. represented by a Timestamp + a timezone)
    * `Date` (e.g. calendar date, represented by three ints, YYYY-MM-DD, not an exact moment in time)
    * `Time` (e.g. time of day, represented by two ints and a float, 23:59:59.999, not an exact moment in time)

* Types
  * Maybe type (e.g. `T?`, `undef`)
  * Result type (e.g. `Ok`, `Err`)

* Collections
  * List (e.g. `[]`, `[x]`, `[x, ..xs]`) - are backed by arrays, not linked-lists
  * Set (e.g. `Set.empty`, `{1}`, `{1..5}`)
  * Dict (e.g. `Dict.empty`, `{ a: 1, b: 2 }`)
  * Tuple (e.g. `(1, 2)`, `(x, y)`)
