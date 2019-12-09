# Classes

I like the idea of "bundling functions with data", especially when used with
ADTs.  This makes a lot of practical sense.  A classic approach for this would
be a "class" with some instance variables and a bunch of class methods,
operating on that internal state.

But a "class" will quickly be associated with "inheritance", which is a concept
that has the potential to lead to complex code when used with more than
a single level of inheritance.

In practice, I've often seen classes work well when used in isolation, to name
things that belong together, and to bundle together all operations that make
sense on a single piece of data.  Bundling methods is also an aid to IDEs, so
they can show a list of useful operations on a particular data structure.

```nox
type Tree =
  | Leaf
  | Node { left: Tree<T>, right: Tree<T> }

@static on Tree
func empty(): Tree<T> {
  return EMPTY
}

@static on Tree
func singleton(value: T): Tree<T> {
  return Node { left: value, right: Leaf() }
}

@on Tree
func left(tree: Tree<T>): Tree<T>? {
  return tree
    | Leaf -> None
    | Node { left } -> left
}

@on Tree
func right(tree: Tree<T>): Tree<T>? {
  return tree
    | Leaf -> None
    | Node { right } -> right
}
```

