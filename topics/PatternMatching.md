# Pattern matching

Pattern matching is supported via the following syntax:

    (expr)
        | pattern1 -> expr1
        | pattern2 -> expr2

Patterns do two things: they check the shape of the value, and "unpack" nested
values by assigning them to (new) local variables that then can be used inside
the scopes on the right hand sides.

For example, matching Ints:

    (value)
        | 0 -> 0
        | 1 -> 1
        | _ -> 2


Matching strings:

    (value)
        | 'foo' -> 1
        | 'bar' -> 2
        | _ -> 3


Matching booleans:

    (value)
        | True -> 1
        | False -> 0

    // Although you'll typically want to write that as:
    (value) ? 1 : 0

You can also unpack enums/ADTs with this:

    (Just(3))
        | Just(x)  -> 'Just #{x}'
        | Nothing() -> 'Nothing'


For example, matching lists:

    ([1, 2, 3])
        | []           -> 'Will NOT match'
        | [x]          -> 'Will NOT match'
        | [x, y]       -> 'Will NOT match'
        | [x, y, z]    -> 'Will match'
        | [_, _, _]    -> 'Will match'
        | [x, y, z, _] -> 'Will NOT match'
        | [x, ..ys]    -> 'Will match'
        | [x, .._]     -> 'Will match'
        | _            -> 'Will match'


You can also match tuples:

    (1, 'hai')
        | (_, _, _)    -> 'Will NOT match'
        | (_, _)       -> 'Will match'
        | (x, _) if x > 0 -> 'Will match'
        | (x, _) if x < 0 -> 'Will NOT match'
        | _            -> 'Will match'




# Pattern matching on iterables

Pattern matching on iterables is especially interesting / advanced.  Since an
iterable must be consumed before it can be tested, the order in which the
patterns are executed is crucial, and the act of pattern matching on an
iterable mutates it.

Here's the reference implementation for `foldr`:

    return (iterable)
        | [] -> start
        | [head] -> f(head, start)
        | [head, ..tail] -> f(head, foldr(tail, f, start))

Notice how we need to handle three cases here.  The JS code that this pattern
match block should execute resembles something like:

```js
function foldr(iterable, f, start) {
  const it = iterable[Symbol.iterator]()
  let buf = it.next()
  if (buf.done) {
    return start
  } else {
    const head = buf.value
    buf = it.next()
    if (buf.done) {
      return f(head, start)
    } else {
      return f(head, foldr(it, f, start))
    }
  }
}
```

When specifying "iterable unpacking", there needs to exist a case that handles
every fixed length explicitly, from 0 to infinity.

For example, the following is invalid, because this pattern does not handle the
case with inputs larger than 0.

    return (iterable)
        | [] -> 0

The following is invalid, too, because this pattern does not handle the
cases with inputs larger than 1.

    return (iterable)
        | [] -> 0
        | [x] -> x

The following is valid, because it specifies all possible cases:

    return (iterable)
        | [] -> 0
        | [x, ..] -> x

    // or

    return (iterable)
        | [] -> 0
        | [x] -> x      // redundant, already covered by case 3
        | [x, ..] -> x

But consider a conditional case on an unpacked value, like below: this would be
invalid, since not all possible cases with a list of 1 item are handled (i.e.
the input `[0]` does not match any rule).

    return (iterable)
        | [] -> 0
        | [x] if x > 0 -> x
        | [x, y, ..] = x + y

Note that pattern matching on an iterable is a mutating action (it potentially
mutates the iterable by changing its internal state).  That is why you cannot
use the `iterable` expression in the exprs on the right hand side.

    return (iterable)
        | [x] -> repeat(x)
        | _   -> iterable   // iterable has been mutated by consuming it (to
                            // test if the first case matched), so you cannot
                            // return it

Instead, you should do this:

    return (iterable)
        | []  -> []         // handle 0-item case
        | [x] -> repeat(x)  // handle 1-item case
        | [x, ..y] -> y     // hnalde 1-or-more case

