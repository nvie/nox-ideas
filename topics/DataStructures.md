# Data Structures

* Document Non-Scalars
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


## Collections

There are 5 collection types in Nox: tuples, lists, sets, dicts, records.

Collections are container types that allow a membership tests with the `in`
operator.

    2 in [1, 2, 3]                  // True
    'hey' in #{ 'hello', 'world' }  // False
    'hey' in #{ a: 'hey' }          // False
    'hey' in #{ 'hey': 123 }        // True
    123 in #{ '123': 'test' }       // False
    123 in #{ 123: 'test' }         // True

