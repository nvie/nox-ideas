# Data Structures

Built-in data structures that are part of the Nox standard library, and have
first-class language syntax support are:

* Scalars (or Primitives)
  * Bool (e.g. `True` | `False`)
  * String (e.g. `'hai'`, `"hello"`, `` `hola` ``)
  * Int (e.g. `1`, `2`, `9`)
  * Float
  * Date

* Types
  * Maybe type (e.g. `T?`, `undef`)
  * Result type (e.g. `Ok`, `Err`)

* Collections
  * List (e.g. `[]`, `[x]`, `[x, ..xs]`) - are backed by arrays, not linked-lists
  * Set (e.g. `Set.empty`, `{1}`, `{1..5}`)
  * Dict (e.g. `Dict.empty`, `{ a: 1, b: 2 }`)
  * Tuple (e.g. `(1, 2)`, `(x, y)`)
