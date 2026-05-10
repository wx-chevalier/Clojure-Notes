# Sequential Collections

Clojure collections "collect" values into compound values. There are four key Clojure collection types: vectors, lists, sets, and maps. Of those four collection types, vectors and lists are ordered.

## Vectors

Vectors are an indexed, sequential data structure. Vectors are represented with `[ ]` like this:

```
[1 2 3]
```

### Indexed access

"Indexed" means that elements of a vector can be retrieved by index. In Clojure (as in Java), indexes start at 0, not 1. Use the `get` function to retrieve an element at an index:

```
user=> (get ["abc" false 99] 0)
"abc"
user=> (get ["abc" false 99] 1)
false
```

Calling get with an invalid index returns `nil`:

```
user=> (get ["abc" false 99] 14)
nil
```

### `count`

All Clojure collections can be counted:

```
user=> (count [1 2 3])
3
```

### Constructing

In addition to the literal `[ ]` syntax, Clojure vectors can be created with the `vector` function:

```
user=> (vector 1 2 3)
[1 2 3]
```

### Adding elements

Elements are added to a vector with `conj` (short for conjoin). Elements are always added to a vector at the end:

```
user=> (conj [1 2 3] 4 5 6)
[1 2 3 4 5 6]
```

### Immutability

Clojure collections share important properties of simple values like strings and numbers, such as immutability and equality comparison by value.

For example, lets create a vector and modify it with `conj`.

```
user=> (def v [1 2 3])
#'user/v
user=> (conj v 4 5 6)
[1 2 3 4 5 6]
```

Here `conj` returned a new vector but if we examine the original vector, we see it’s unchanged:

```
user=> v
[1 2 3]
```

Any function that "changes" a collection returns a new instance. Your program will need to remember or pass along the changed instance to take advantage of it.

## Lists

Lists are sequential linked lists that add new elements at the head of the list, instead of at the tail like vectors.

### Constructing

Because lists are evaluated by invoking the first element as a function, we must quote a list to prevent evaluation:

```
(def cards '(10 :ace :jack 9))
```

Lists are not indexed so they must be walked using `first` and `rest`.

```
user=> (first cards)
10
user=> (rest cards)
'(:ace :jack 9)
```

### Adding elements

`conj` can be used to add elements to a list just as with vectors. However, `conj` always adds elements where it can be done in constant time for the data structure. In the case of lists, elements are added at the front:

```
user=> (conj cards :queen)
(:queen 10 :ace :jack 9)
```

### Stack access

Lists can also be used as a stack with peek and pop:

```
user=> (def stack '(:a :b))
#'user/stack
user=> (peek stack)
:a
user=> (pop stack)
(:b)
```
