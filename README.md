# Content

This crate implements the `LendIterator` and `LendIteratorMut` traits
in a hopefully generic enough way.

`LendIterator` allows a collection of elements to generate iterators that borrow
the data in the collection.
The references that these iterators yield can have a lifetime that is shorter than
the lifetime of the collection, but many such iterators can be created
during the lifespan of the collection.

`LendIteratorMut` is similar to `LendIterator`, but the item returned from it
is a bit more complex, as it has to contain a mutable reference to some data from
the collection, and can optionally contain an immutable reference to other pieces
of data from the same collection.  
An example of when this added complexity comes handy is when one has to implement
an iterator over items that contain some data that is subject to mantaining some
invariants, as it is the case with maps.  
Indeed, a mutable iterator on a map returns a pair of references. The first reference
is immutable and refers to the the key in the map, and the second one is
a mutable reference to the corresponding value.

## Custom implementations and safety

Unfortunately, to this day (Aug 2023), in most cases Rust does not allow
the safe implementation of some of the operations required to implement
this trait for a collection.  
More specifically, some lifetime extensions are needed at some point,
and even if those happen to be safe,
it is the programmer who has to guarantee they are.

If Rust was to add some features that make the language more expressive, such
as more flexible, but still sound, lifetime rules,
`LendIterator` and `LendIteratorMut` could be safely implemented.
